
# 🎮 MANUAL DE ESTRUTURAS DE DADOS PARA JOGOS NO TERMINAL EM PYTHON

## 📋 ÍNDICE
1. [Estruturas Fundamentais para Jogos](#estruturas-fundamentais-para-jogos)
2. [Representação do Mapa](#representação-do-mapa)
3. [Sistemas de Grid](#sistemas-de-grid)
4. [Entidades e Personagens](#entidades-e-personagens)
5. [Inventário e Itens](#inventário-e-itens)
6. [Sistemas de Combate](#sistemas-de-combate)
7. [Quest e Progressão](#quest-e-progressão)
8. [Salvamento de Jogo](#salvamento-de-jogo)
9. [IA e Pathfinding](#ia-e-pathfinding)
10. [Efeitos Visuais no Terminal](#efeitos-visuais-no-terminal)
11. [Exemplos Completos](#exemplos-completos)
12. [Padrões de Design](#padrões-de-design)

---

## 🏗️ ESTRUTURAS FUNDAMENTAIS PARA JOGOS

### 1. Vector2D para Coordenadas
```python
from dataclasses import dataclass
from typing import Tuple

@dataclass
class Vector2D:
    """Representa coordenadas 2D para jogos no terminal"""
    x: int
    y: int
    
    def __add__(self, other: 'Vector2D') -> 'Vector2D':
        return Vector2D(self.x + other.x, self.y + other.y)
    
    def __sub__(self, other: 'Vector2D') -> 'Vector2D':
        return Vector2D(self.x - other.x, self.y - other.y)
    
    def __eq__(self, other: 'Vector2D') -> bool:
        return self.x == other.x and self.y == other.y
    
    def distance_to(self, other: 'Vector2D') -> float:
        """Distância Euclidiana entre dois pontos"""
        return ((self.x - other.x) ** 2 + (self.y - other.y) ** 2) ** 0.5
    
    def manhattan_distance(self, other: 'Vector2D') -> int:
        """Distância Manhattan (útil para grids)"""
        return abs(self.x - other.x) + abs(self.y - other.y)
    
    def to_tuple(self) -> Tuple[int, int]:
        return (self.x, self.y)
    
    @classmethod
    def from_tuple(cls, t: Tuple[int, int]) -> 'Vector2D':
        return cls(t[0], t[1])
    
    def clamp(self, min_x: int, max_x: int, min_y: int, max_y: int) -> 'Vector2D':
        """Mantém o vetor dentro dos limites"""
        return Vector2D(
            max(min_x, min(self.x, max_x)),
            max(min_y, min(self.y, max_y))
        )

# Uso
posicao = Vector2D(5, 10)
nova_pos = posicao + Vector2D(1, -1)  # (6, 9)
distancia = posicao.distance_to(Vector2D(0, 0))
```

### 2. Classe Base para Entidades
```python
from typing import Optional, List, Dict, Any
from enum import Enum

class EntityType(Enum):
    """Tipos de entidades no jogo"""
    PLAYER = "player"
    ENEMY = "enemy"
    NPC = "npc"
    ITEM = "item"
    CONTAINER = "container"
    DOOR = "door"
    TRAP = "trap"

class Entity:
    """Classe base para todas as entidades do jogo"""
    
    def __init__(self, 
                 entity_id: str,
                 name: str,
                 entity_type: EntityType,
                 position: Vector2D,
                 symbol: str = "?",
                 color: str = "white"):
        
        self.id = entity_id
        self.name = name
        self.type = entity_type
        self.position = position
        self.symbol = symbol
        self.color = color
        self.components: Dict[str, Any] = {}
        self.tags: List[str] = []
        
    def add_component(self, name: str, component: Any):
        """Sistema de componentes para comportamento flexível"""
        self.components[name] = component
    
    def get_component(self, name: str) -> Optional[Any]:
        return self.components.get(name)
    
    def has_component(self, name: str) -> bool:
        return name in self.components
    
    def has_tag(self, tag: str) -> bool:
        return tag in self.tags
    
    def move(self, direction: Vector2D):
        """Move a entidade na direção especificada"""
        self.position += direction
    
    def __str__(self) -> str:
        return f"{self.name} ({self.type.value}) at {self.position}"
    
    def to_dict(self) -> Dict[str, Any]:
        """Serialização para salvamento"""
        return {
            'id': self.id,
            'name': self.name,
            'type': self.type.value,
            'position': self.position.to_tuple(),
            'symbol': self.symbol,
            'color': self.color,
            'tags': self.tags,
            'components': {
                name: comp.to_dict() if hasattr(comp, 'to_dict') else str(comp)
                for name, comp in self.components.items()
            }
        }
    
    @classmethod
    def from_dict(cls, data: Dict[str, Any]) -> 'Entity':
        """Desserialização do salvamento"""
        entity = cls(
            entity_id=data['id'],
            name=data['name'],
            entity_type=EntityType(data['type']),
            position=Vector2D.from_tuple(data['position']),
            symbol=data.get('symbol', '?'),
            color=data.get('color', 'white')
        )
        entity.tags = data.get('tags', [])
        return entity
```

### 3. Sistema de Eventos
```python
from typing import Callable, Dict, List, Any
from dataclasses import dataclass
from datetime import datetime

@dataclass
class GameEvent:
    """Estrutura para eventos do jogo"""
    event_type: str
    source: Any  # Quem causou o evento
    target: Any  # Quem recebe o evento
    data: Dict[str, Any]
    timestamp: datetime
    
    def __init__(self, event_type: str, source=None, target=None, **kwargs):
        self.event_type = event_type
        self.source = source
        self.target = target
        self.data = kwargs
        self.timestamp = datetime.now()

class EventSystem:
    """Sistema de pub/sub para eventos do jogo"""
    
    def __init__(self):
        self._listeners: Dict[str, List[Callable]] = {}
        self._event_queue: List[GameEvent] = []
        
    def subscribe(self, event_type: str, callback: Callable):
        """Registra um listener para um tipo de evento"""
        if event_type not in self._listeners:
            self._listeners[event_type] = []
        self._listeners[event_type].append(callback)
    
    def unsubscribe(self, event_type: str, callback: Callable):
        """Remove um listener"""
        if event_type in self._listeners:
            self._listeners[event_type].remove(callback)
    
    def emit(self, event: GameEvent):
        """Envia um evento para processamento imediato"""
        if event.event_type in self._listeners:
            for callback in self._listeners[event.event_type]:
                callback(event)
    
    def queue(self, event: GameEvent):
        """Adiciona evento à fila para processamento posterior"""
        self._event_queue.append(event)
    
    def process_queue(self):
        """Processa todos os eventos na fila"""
        while self._event_queue:
            event = self._event_queue.pop(0)
            self.emit(event)
    
    def clear_queue(self):
        """Limpa a fila de eventos"""
        self._event_queue.clear()

# Exemplo de uso
event_system = EventSystem()

def on_player_move(event: GameEvent):
    print(f"Player moved to {event.data.get('position')}")

event_system.subscribe("player_move", on_player_move)
```

---

## 🗺️ REPRESENTAÇÃO DO MAPA

### 1. Tile System
```python
from typing import Optional, Dict, Any
from enum import Enum
import json

class TileType(Enum):
    """Tipos de tiles no mapa"""
    FLOOR = "floor"
    WALL = "wall"
    WATER = "water"
    LAVA = "lava"
    GRASS = "grass"
    DOOR_OPEN = "door_open"
    DOOR_CLOSED = "door_closed"
    STAIRS_UP = "stairs_up"
    STAIRS_DOWN = "stairs_down"

class Tile:
    """Representa uma célula no mapa"""
    
    TILE_SYMBOLS = {
        TileType.FLOOR: ".",
        TileType.WALL: "#",
        TileType.WATER: "~",
        TileType.LAVA: "&",
        TileType.GRASS: '"',
        TileType.DOOR_OPEN: "/",
        TileType.DOOR_CLOSED: "+",
        TileType.STAIRS_UP: "<",
        TileType.STAIRS_DOWN: ">",
    }
    
    TILE_COLORS = {
        TileType.FLOOR: "white",
        TileType.WALL: "grey",
        TileType.WATER: "blue",
        TileType.LAVA: "red",
        TileType.GRASS: "green",
        TileType.DOOR_OPEN: "yellow",
        TileType.DOOR_CLOSED: "brown",
        TileType.STAIRS_UP: "cyan",
        TileType.STAIRS_DOWN: "magenta",
    }
    
    def __init__(self, 
                 tile_type: TileType,
                 walkable: bool = True,
                 transparent: bool = True,
                 discovered: bool = False,
                 visible: bool = False,
                 data: Optional[Dict[str, Any]] = None):
        
        self.tile_type = tile_type
        self.walkable = walkable
        self.transparent = transparent
        self.discovered = discovered
        self.visible = visible
        self.data = data or {}
        self.entity: Optional[Entity] = None
        self.items: List['Item'] = []
    
    @property
    def symbol(self) -> str:
        """Símbolo para renderização"""
        if self.entity:
            return self.entity.symbol
        return self.TILE_SYMBOLS.get(self.tile_type, "?")
    
    @property
    def color(self) -> str:
        """Cor para renderização"""
        if self.entity:
            return self.entity.color
        return self.TILE_COLORS.get(self.tile_type, "white")
    
    def add_item(self, item: 'Item'):
        """Adiciona um item ao tile"""
        self.items.append(item)
    
    def remove_item(self, item: 'Item') -> bool:
        """Remove um item do tile"""
        if item in self.items:
            self.items.remove(item)
            return True
        return False
    
    def get_items(self) -> List['Item']:
        """Retorna todos os items no tile"""
        return self.items.copy()
    
    def to_dict(self) -> Dict[str, Any]:
        """Serialização para salvamento"""
        return {
            'tile_type': self.tile_type.value,
            'walkable': self.walkable,
            'transparent': self.transparent,
            'discovered': self.discovered,
            'visible': self.visible,
            'data': self.data,
            'items': [item.to_dict() for item in self.items]
        }
    
    @classmethod
    def from_dict(cls, data: Dict[str, Any]) -> 'Tile':
        """Desserialização do salvamento"""
        tile = cls(
            tile_type=TileType(data['tile_type']),
            walkable=data['walkable'],
            transparent=data['transparent'],
            discovered=data.get('discovered', False),
            visible=data.get('visible', False),
            data=data.get('data', {})
        )
        
        from items import Item  # Importação local para evitar circular
        tile.items = [Item.from_dict(item_data) for item_data in data.get('items', [])]
        
        return tile
```

### 2. Map Grid
```python
from typing import List, Optional, Tuple, Generator
import random

class GameMap:
    """Grid que representa o mapa do jogo"""
    
    def __init__(self, width: int, height: int, level: int = 1):
        self.width = width
        self.height = height
        self.level = level
        self.tiles: List[List[Tile]] = []
        self.entities: Dict[str, Entity] = {}
        self.player_start: Optional[Vector2D] = None
        self.stairs_up: Optional[Vector2D] = None
        self.stairs_down: Optional[Vector2D] = None
        
        self._initialize_tiles()
    
    def _initialize_tiles(self):
        """Inicializa a grid com tiles vazios"""
        self.tiles = [
            [Tile(TileType.WALL, walkable=False, transparent=False) 
             for _ in range(self.width)]
            for _ in range(self.height)
        ]
    
    def generate_dungeon(self, 
                        room_min_size: int = 3,
                        room_max_size: int = 10,
                        max_rooms: int = 30):
        """Gera um dungeon procedural usando algoritmo de salas e corredores"""
        
        rooms: List[Dict[str, Any]] = []
        
        for _ in range(max_rooms):
            # Gera dimensões aleatórias para a sala
            room_width = random.randint(room_min_size, room_max_size)
            room_height = random.randint(room_min_size, room_max_size)
            
            # Posição aleatória
            x = random.randint(1, self.width - room_width - 2)
            y = random.randint(1, self.height - room_height - 2)
            
            new_room = {
                'x': x, 'y': y,
                'width': room_width, 'height': room_height
            }
            
            # Verifica se a sala não intersecta outras
            if any(self._rooms_intersect(new_room, room) for room in rooms):
                continue
            
            # Cria a sala
            self._create_room(new_room)
            
            # Conecta com a sala anterior
            if rooms:
                prev_room = rooms[-1]
                self._create_tunnel_between(prev_room, new_room)
            
            rooms.append(new_room)
        
        # Define posições importantes
        if rooms:
            self.player_start = Vector2D(
                rooms[0]['x'] + rooms[0]['width'] // 2,
                rooms[0]['y'] + rooms[0]['height'] // 2
            )
            
            last_room = rooms[-1]
            self.stairs_down = Vector2D(
                last_room['x'] + last_room['width'] // 2,
                last_room['y'] + last_room['height'] // 2
            )
            
            # Adiciona as escadas
            self.set_tile(self.stairs_down.x, self.stairs_down.y, 
                         Tile(TileType.STAIRS_DOWN))
    
    def _create_room(self, room: Dict[str, Any]):
        """Cria uma sala retangular"""
        for x in range(room['x'] + 1, room['x'] + room['width']):
            for y in range(room['y'] + 1, room['y'] + room['height']):
                self.set_tile(x, y, Tile(TileType.FLOOR))
    
    def _create_tunnel_between(self, room1: Dict[str, Any], room2: Dict[str, Any]):
        """Cria um túnel entre duas salas"""
        x1, y1 = room1['x'] + room1['width'] // 2, room1['y'] + room1['height'] // 2
        x2, y2 = room2['x'] + room2['width'] // 2, room2['y'] + room2['height'] // 2
        
        # 50% de chance de começar horizontal ou vertical
        if random.random() < 0.5:
            # Horizontal primeiro, depois vertical
            self._create_h_tunnel(x1, x2, y1)
            self._create_v_tunnel(y1, y2, x2)
        else:
            # Vertical primeiro, depois horizontal
            self._create_v_tunnel(y1, y2, x1)
            self._create_h_tunnel(x1, x2, y2)
    
    def _create_h_tunnel(self, x1: int, x2: int, y: int):
        """Cria túnel horizontal"""
        for x in range(min(x1, x2), max(x1, x2) + 1):
            self.set_tile(x, y, Tile(TileType.FLOOR))
    
    def _create_v_tunnel(self, y1: int, y2: int, x: int):
        """Cria túnel vertical"""
        for y in range(min(y1, y2), max(y1, y2) + 1):
            self.set_tile(x, y, Tile(TileType.FLOOR))
    
    def _rooms_intersect(self, room1: Dict[str, Any], room2: Dict[str, Any]) -> bool:
        """Verifica se duas salas se intersectam"""
        return (room1['x'] <= room2['x'] + room2['width'] and
                room1['x'] + room1['width'] >= room2['x'] and
                room1['y'] <= room2['y'] + room2['height'] and
                room1['y'] + room1['height'] >= room2['y'])
    
    def set_tile(self, x: int, y: int, tile: Tile):
        """Define um tile em uma posição específica"""
        if self.is_in_bounds(x, y):
            self.tiles[y][x] = tile
    
    def get_tile(self, x: int, y: int) -> Optional[Tile]:
        """Obtém o tile em uma posição específica"""
        if self.is_in_bounds(x, y):
            return self.tiles[y][x]
        return None
    
    def is_in_bounds(self, x: int, y: int) -> bool:
        """Verifica se as coordenadas estão dentro do mapa"""
        return 0 <= x < self.width and 0 <= y < self.height
    
    def is_walkable(self, x: int, y: int) -> bool:
        """Verifica se uma posição é transitável"""
        tile = self.get_tile(x, y)
        if not tile or not tile.walkable:
            return False
        
        # Verifica se há entidades bloqueando
        entity = self.get_entity_at(x, y)
        if entity and entity.has_tag('blocking'):
            return False
        
        return True
    
    def get_entity_at(self, x: int, y: int) -> Optional[Entity]:
        """Retorna a entidade em uma posição específica"""
        for entity in self.entities.values():
            if entity.position.x == x and entity.position.y == y:
                return entity
        return None
    
    def add_entity(self, entity: Entity) -> bool:
        """Adiciona uma entidade ao mapa"""
        if not self.is_in_bounds(entity.position.x, entity.position.y):
            return False
        
        self.entities[entity.id] = entity
        return True
    
    def remove_entity(self, entity_id: str) -> bool:
        """Remove uma entidade do mapa"""
        if entity_id in self.entities:
            del self.entities[entity_id]
            return True
        return False
    
    def move_entity(self, entity_id: str, new_pos: Vector2D) -> bool:
        """Move uma entidade para uma nova posição"""
        if entity_id not in self.entities:
            return False
        
        if not self.is_walkable(new_pos.x, new_pos.y):
            return False
        
        entity = self.entities[entity_id]
        entity.position = new_pos
        return True
    
    def get_neighbors(self, x: int, y: int, 
                     include_diagonals: bool = False) -> List[Tuple[int, int]]:
        """Obtém as posições vizinhas"""
        neighbors = []
        directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]  # Cardinal directions
        
        if include_diagonals:
            directions += [(-1, -1), (-1, 1), (1, -1), (1, 1)]
        
        for dx, dy in directions:
            nx, ny = x + dx, y + dy
            if self.is_in_bounds(nx, ny):
                neighbors.append((nx, ny))
        
        return neighbors
    
    def to_dict(self) -> Dict[str, Any]:
        """Serialização para salvamento"""
        return {
            'width': self.width,
            'height': self.height,
            'level': self.level,
            'tiles': [
                [tile.to_dict() for tile in row]
                for row in self.tiles
            ],
            'entities': {
                entity_id: entity.to_dict()
                for entity_id, entity in self.entities.items()
            },
            'player_start': self.player_start.to_tuple() if self.player_start else None,
            'stairs_up': self.stairs_up.to_tuple() if self.stairs_up else None,
            'stairs_down': self.stairs_down.to_tuple() if self.stairs_down else None
        }
    
    @classmethod
    def from_dict(cls, data: Dict[str, Any]) -> 'GameMap':
        """Desserialização do salvamento"""
        game_map = cls(
            width=data['width'],
            height=data['height'],
            level=data['level']
        )
        
        # Carrega tiles
        game_map.tiles = [
            [Tile.from_dict(tile_data) for tile_data in row]
            for row in data['tiles']
        ]
        
        # Carrega entidades
        from entities import Entity  # Importação local
        game_map.entities = {
            entity_id: Entity.from_dict(entity_data)
            for entity_id, entity_data in data['entities'].items()
        }
        
        # Carrega posições importantes
        if data.get('player_start'):
            game_map.player_start = Vector2D.from_tuple(data['player_start'])
        if data.get('stairs_up'):
            game_map.stairs_up = Vector2D.from_tuple(data['stairs_up'])
        if data.get('stairs_down'):
            game_map.stairs_down = Vector2D.from_tuple(data['stairs_down'])
        
        return game_map
```

---

## 🎭 ENTIDADES E PERSONAGENS

### 1. Componentes para Entidades
```python
from typing import Optional, Dict, Any
from abc import ABC, abstractmethod

class Component(ABC):
    """Classe base para componentes de entidades"""
    
    def __init__(self, owner: Entity):
        self.owner = owner
    
    @abstractmethod
    def update(self, game_map: GameMap, event_system: EventSystem):
        """Atualiza o componente"""
        pass
    
    def to_dict(self) -> Dict[str, Any]:
        """Serialização do componente"""
        return {
            'type': self.__class__.__name__
        }
    
    @classmethod
    def from_dict(cls, data: Dict[str, Any], owner: Entity) -> 'Component':
        """Desserialização do componente"""
        return cls(owner)

class HealthComponent(Component):
    """Componente para gerenciar vida"""
    
    def __init__(self, owner: Entity, max_hp: int = 100):
        super().__init__(owner)
        self.max_hp = max_hp
        self.current_hp = max_hp
        self.is_alive = True
    
    def take_damage(self, amount: int, source: Optional[Entity] = None) -> bool:
        """Aplica dano à entidade"""
        self.current_hp -= amount
        self.current_hp = max(0, self.current_hp)
        
        if self.current_hp <= 0:
            self.is_alive = False
            if source:
                event_system.emit(GameEvent(
                    "entity_died",
                    source=source,
                    target=self.owner,
                    damage_amount=amount
                ))
        
        return self.is_alive
    
    def heal(self, amount: int) -> int:
        """Cura a entidade"""
        old_hp = self.current_hp
        self.current_hp = min(self.max_hp, self.current_hp + amount)
        return self.current_hp - old_hp
    
    def get_health_percentage(self) -> float:
        """Retorna porcentagem de vida"""
        return self.current_hp / self.max_hp
    
    def update(self, game_map: GameMap, event_system: EventSystem):
        """Atualiza o componente de vida"""
        pass  # Componente passivo
    
    def to_dict(self) -> Dict[str, Any]:
        data = super().to_dict()
        data.update({
            'max_hp': self.max_hp,
            'current_hp': self.current_hp,
            'is_alive': self.is_alive
        })
        return data
    
    @classmethod
    def from_dict(cls, data: Dict[str, Any], owner: Entity) -> 'HealthComponent':
        comp = cls(owner, data['max_hp'])
        comp.current_hp = data['current_hp']
        comp.is_alive = data['is_alive']
        return comp

class CombatComponent(Component):
    """Componente para combate"""
    
    def __init__(self, owner: Entity,
                 attack_power: int = 10,
                 defense: int = 5,
                 attack_range: int = 1):
        super().__init__(owner)
        self.attack_power = attack_power
        self.defense = defense
        self.attack_range = attack_range
        self.critical_chance = 0.1  # 10%
        self.critical_multiplier = 2.0
    
    def attack(self, target: Entity) -> Dict[str, Any]:
        """Executa um ataque contra um alvo"""
        # Verifica se o alvo tem componente de vida
        target_health = target.get_component('health')
        if not target_health:
            return {'success': False, 'reason': 'target_has_no_health'}
        
        # Calcula dano
        damage = self.attack_power
        
        # Chance de crítico
        import random
        if random.random() < self.critical_chance:
            damage = int(damage * self.critical_multiplier)
            is_critical = True
        else:
            is_critical = False
        
        # Aplica defesa do alvo
        target_defense = 0
        target_combat = target.get_component('combat')
        if target_combat:
            damage = max(1, damage - target_combat.defense)
        
        # Aplica dano
        target_health.take_damage(damage, self.owner)
        
        return {
            'success': True,
            'damage': damage,
            'is_critical': is_critical,
            'target_alive': target_health.is_alive
        }
    
    def update(self, game_map: GameMap, event_system: EventSystem):
        pass
    
    def to_dict(self) -> Dict[str, Any]:
        data = super().to_dict()
        data.update({
            'attack_power': self.attack_power,
            'defense': self.defense,
            'attack_range': self.attack_range,
            'critical_chance': self.critical_chance,
            'critical_multiplier': self.critical_multiplier
        })
        return data
    
    @classmethod
    def from_dict(cls, data: Dict[str, Any], owner: Entity) -> 'CombatComponent':
        comp = cls(
            owner,
            attack_power=data['attack_power'],
            defense=data['defense'],
            attack_range=data['attack_range']
        )
        comp.critical_chance = data['critical_chance']
        comp.critical_multiplier = data['critical_multiplier']
        return comp

class AIComponent(Component):
    """Componente para inteligência artificial"""
    
    def __init__(self, owner: Entity, 
                 ai_type: str = "basic",
                 aggro_range: int = 8,
                 sight_range: int = 5):
        super().__init__(owner)
        self.ai_type = ai_type  # basic, coward, aggressive, etc.
        self.aggro_range = aggro_range
        self.sight_range = sight_range
        self.target: Optional[Entity] = None
        self.state = "idle"  # idle, chasing, attacking, fleeing
        self.path: List[Vector2D] = []
    
    def update(self, game_map: GameMap, event_system: EventSystem):
        """Atualiza a IA da entidade"""
        # Busca o jogador
        player = game_map.get_entity_by_type(EntityType.PLAYER)
        if not player:
            return
        
        # Calcula distância para o jogador
        distance = self.owner.position.distance_to(player.position)
        
        # Máquina de estados da IA
        if self.ai_type == "basic":
            self._basic_ai(game_map, player, distance)
        elif self.ai_type == "coward":
            self._coward_ai(game_map, player, distance)
        elif self.ai_type == "aggressive":
            self._aggressive_ai(game_map, player, distance)
    
    def _basic_ai(self, game_map: GameMap, player: Entity, distance: float):
        """IA básica que segue e ataca o jogador"""
        if distance <= self.sight_range:
            self.state = "chasing"
            self.target = player
            
            # Se está perto o suficiente, ataca
            if distance <= 1.5:  # Adjacente
                combat = self.owner.get_component('combat')
                if combat:
                    combat.attack(player)
                self.state = "attacking"
            else:
                # Move-se em direção ao jogador
                self._move_towards(game_map, player.position)
        else:
            self.state = "idle"
            self.target = None
    
    def _coward_ai(self, game_map: GameMap, player: Entity, distance: float):
        """IA que foge do jogador quando ferida"""
        health = self.owner.get_component('health')
        if not health:
            return
        
        # Se está com pouca vida, foge
        if health.get_health_percentage() < 0.3:
            self.state = "fleeing"
            self._move_away(game_map, player.position)
        else:
            # Se não, comportamento normal
            self._basic_ai(game_map, player, distance)
    
    def _aggressive_ai(self, game_map: GameMap, player: Entity, distance: float):
        """IA que é sempre agressiva"""
        if distance <= self.aggro_range:
            self.state = "chasing"
            self.target = player
            
            # Tenta atacar a qualquer distância
            combat = self.owner.get_component('combat')
            if combat and distance <= combat.attack_range:
                combat.attack(player)
                self.state = "attacking"
            else:
                self._move_towards(game_map, player.position)
    
    def _move_towards(self, game_map: GameMap, target_pos: Vector2D):
        """Move a entidade em direção ao alvo"""
        from pathfinding import a_star  # Importação local
        
        # Usa A* para encontrar caminho
        path = a_star(
            game_map,
            self.owner.position.to_tuple(),
            target_pos.to_tuple()
        )
        
        if len(path) > 1:
            next_pos = Vector2D.from_tuple(path[1])
            if game_map.is_walkable(next_pos.x, next_pos.y):
                game_map.move_entity(self.owner.id, next_pos)
    
    def _move_away(self, game_map: GameMap, from_pos: Vector2D):
        """Move a entidade para longe de uma posição"""
        # Encontra a direção oposta
        dx = self.owner.position.x - from_pos.x
        dy = self.owner.position.y - from_pos.y
        
        # Normaliza a direção
        if dx != 0:
            dx = 1 if dx > 0 else -1
        if dy != 0:
            dy = 1 if dy > 0 else -1
        
        # Tenta mover-se
        new_pos = self.owner.position + Vector2D(dx, dy)
        if game_map.is_walkable(new_pos.x, new_pos.y):
            game_map.move_entity(self.owner.id, new_pos)
    
    def to_dict(self) -> Dict[str, Any]:
        data = super().to_dict()
        data.update({
            'ai_type': self.ai_type,
            'aggro_range': self.aggro_range,
            'sight_range': self.sight_range,
            'state': self.state,
            'path': [pos.to_tuple() for pos in self.path] if self.path else []
        })
        return data
    
    @classmethod
    def from_dict(cls, data: Dict[str, Any], owner: Entity) -> 'AIComponent':
        comp = cls(
            owner,
            ai_type=data['ai_type'],
            aggro_range=data['aggro_range'],
            sight_range=data['sight_range']
        )
        comp.state = data['state']
        comp.path = [Vector2D.from_tuple(pos) for pos in data.get('path', [])]
        return comp
```

### 2. Fábrica de Entidades
```python
import uuid
from typing import Dict, Any

class EntityFactory:
    """Fábrica para criar entidades com configurações pré-definidas"""
    
    @staticmethod
    def create_player(name: str, position: Vector2D) -> Entity:
        """Cria uma entidade do jogador"""
        player = Entity(
            entity_id=f"player_{uuid.uuid4().hex[:8]}",
            name=name,
            entity_type=EntityType.PLAYER,
            position=position,
            symbol="@",
            color="yellow"
        )
        
        # Adiciona componentes
        player.add_component('health', HealthComponent(player, max_hp=100))
        player.add_component('combat', CombatComponent(player, attack_power=15, defense=8))
        player.add_component('inventory', InventoryComponent(player, capacity=20))
        
        # Adiciona tags
        player.tags.extend(['player', 'blocking', 'controllable'])
        
        return player
    
    @staticmethod
    def create_goblin(position: Vector2D) -> Entity:
        """Cria um inimigo goblin"""
        goblin = Entity(
            entity_id=f"goblin_{uuid.uuid4().hex[:8]}",
            name="Goblin",
            entity_type=EntityType.ENEMY,
            position=position,
            symbol="g",
            color="green"
        )
        
        goblin.add_component('health', HealthComponent(goblin, max_hp=30))
        goblin.add_component('combat', CombatComponent(goblin, attack_power=8, defense=3))
        goblin.add_component('ai', AIComponent(goblin, ai_type="basic"))
        
        goblin.tags.extend(['enemy', 'blocking', 'hostile'])
        
        return goblin
    
    @staticmethod
    def create_healing_potion(position: Vector2D) -> Entity:
        """Cria uma poção de cura"""
        potion = Entity(
            entity_id=f"potion_{uuid.uuid4().hex[:8]}",
            name="Poção de Cura",
            entity_type=EntityType.ITEM,
            position=position,
            symbol="!",
            color="red"
        )
        
        # Componente de item
        potion.add_component('item', ItemComponent(
            potion,
            item_type="consumable",
            use_effect="heal:20"
        ))
        
        potion.tags.extend(['item', 'consumable', 'pickupable'])
        
        return potion
    
    @staticmethod
    def create_chest(position: Vector2D, locked: bool = False) -> Entity:
        """Cria um baú"""
        chest = Entity(
            entity_id=f"chest_{uuid.uuid4().hex[:8]}",
            name="Baú",
            entity_type=EntityType.CONTAINER,
            position=position,
            symbol="=",
            color="brown"
        )
        
        chest.add_component('container', ContainerComponent(chest, capacity=5, locked=locked))
        
        if locked:
            chest.tags.extend(['container', 'locked', 'blocking'])
        else:
            chest.tags.extend(['container', 'blocking'])
        
        return chest
    
    @staticmethod
    def from_config(config: Dict[str, Any]) -> Entity:
        """Cria uma entidade a partir de configuração"""
        entity_type = EntityType(config.get('type', 'enemy'))
        
        entity = Entity(
            entity_id=f"{entity_type.value}_{uuid.uuid4().hex[:8]}",
            name=config.get('name', 'Unnamed'),
            entity_type=entity_type,
            position=Vector2D.from_tuple(config.get('position', (0, 0))),
            symbol=config.get('symbol', '?'),
            color=config.get('color', 'white')
        )
        
        # Adiciona componentes baseados na configuração
        for comp_name, comp_config in config.get('components', {}).items():
            component_class = globals().get(f"{comp_config['type']}Component")
            if component_class:
                comp = component_class.from_dict(comp_config, entity)
                entity.add_component(comp_name, comp)
        
        # Adiciona tags
        entity.tags.extend(config.get('tags', []))
        
        return entity
```

---

## 🎒 INVENTÁRIO E ITENS

### 1. Sistema de Itens
```python
from typing import Optional, List, Dict, Any
from enum import Enum

class ItemRarity(Enum):
    """Raridade dos itens"""
    COMMON = "common"
    UNCOMMON = "uncommon"
    RARE = "rare"
    EPIC = "epic"
    LEGENDARY = "legendary"

class ItemCategory(Enum):
    """Categorias de itens"""
    WEAPON = "weapon"
    ARMOR = "armor"
    CONSUMABLE = "consumable"
    MATERIAL = "material"
    QUEST = "quest"
    KEY = "key"
    MISC = "misc"

class Item:
    """Representa um item no jogo"""
    
    def __init__(self,
                 item_id: str,
                 name: str,
                 category: ItemCategory,
                 symbol: str = "*",
                 color: str = "white",
                 weight: float = 1.0,
                 value: int = 10,
                 rarity: ItemRarity = ItemRarity.COMMON,
                 data: Optional[Dict[str, Any]] = None):
        
        self.id = item_id
        self.name = name
        self.category = category
        self.symbol = symbol
        self.color = color
        self.weight = weight
        self.value = value
        self.rarity = rarity
        self.data = data or {}
        self.stackable = False
        self.stack_size = 1
        self.current_stack = 1
    
    def use(self, user: Entity, target: Optional[Entity] = None) -> Dict[str, Any]:
        """Usa o item"""
        if self.category == ItemCategory.CONSUMABLE:
            return self._use_consumable(user, target)
        elif self.category == ItemCategory.WEAPON:
            return self._use_weapon(user, target)
        elif self.category == ItemCategory.ARMOR:
            return self._equip_armor(user)
        
        return {'success': False, 'message': 'Item não pode ser usado'}
    
    def _use_consumable(self, user: Entity, target: Optional[Entity] = None) -> Dict[str, Any]:
        """Usa um item consumível"""
        target = target or user
        
        # Verifica efeitos no data
        if 'effect' in self.data:
            effect = self.data['effect']
            
            if effect.startswith('heal:'):
                amount = int(effect.split(':')[1])
                health = target.get_component('health')
                if health:
                    healed = health.heal(amount)
                    return {
                        'success': True,
                        'message': f'{target.name} recuperou {healed} de vida',
                        'healed': healed
                    }
            
            elif effect.startswith('damage:'):
                amount = int(effect.split(':')[1])
                health = target.get_component('health')
                if health:
                    health.take_damage(amount, user)
                    return {
                        'success': True,
                        'message': f'{target.name} sofreu {amount} de dano',
                        'damage': amount
                    }
        
        return {'success': False, 'message': 'Efeito desconhecido'}
    
    def _use_weapon(self, user: Entity, target: Optional[Entity] = None) -> Dict[str, Any]:
        """Usa uma arma"""
        if not target:
            return {'success': False, 'message': 'Nenhum alvo especificado'}
        
        combat = user.get_component('combat')
        if not combat:
            return {'success': False, 'message': 'Usuário não pode combater'}
        
        # Aplica bônus da arma
        original_power = combat.attack_power
        if 'attack_bonus' in self.data:
            combat.attack_power += self.data['attack_bonus']
        
        result = combat.attack(target)
        
        # Restaura valor original
        combat.attack_power = original_power
        
        return result
    
    def _equip_armor(self, user: Entity) -> Dict[str, Any]:
        """Equipa uma armadura"""
        combat = user.get_component('combat')
        if not combat:
            return {'success': False, 'message': 'Usuário não pode equipar armadura'}
        
        if 'defense_bonus' in self.data:
            combat.defense += self.data['defense_bonus']
            return {
                'success': True,
                'message': f'{user.name} equipou {self.name}',
                'defense_bonus': self.data['defense_bonus']
            }
        
        return {'success': False, 'message': 'Armadura sem bônus de defesa'}
    
    def can_stack_with(self, other: 'Item') -> bool:
        """Verifica se pode ser empilhado com outro item"""
        return (self.stackable and 
                self.id == other.id and 
                self.current_stack < self.stack_size)
    
    def split_stack(self, amount: int) -> Optional['Item']:
        """Divide um stack de itens"""
        if not self.stackable or amount >= self.current_stack:
            return None
        
        new_item = Item(
            item_id=self.id,
            name=self.name,
            category=self.category,
            symbol=self.symbol,
            color=self.color,
            weight=self.weight,
            value=self.value,
            rarity=self.rarity,
            data=self.data.copy()
        )
        new_item.stackable = self.stackable
        new_item.stack_size = self.stack_size
        new_item.current_stack = amount
        
        self.current_stack -= amount
        
        return new_item
    
    def merge_stack(self, other: 'Item') -> int:
        """Junta dois stacks de itens"""
        if not self.can_stack_with(other):
            return 0
        
        available_space = self.stack_size - self.current_stack
        transfer_amount = min(available_space, other.current_stack)
        
        self.current_stack += transfer_amount
        other.current_stack -= transfer_amount
        
        return transfer_amount
    
    def to_dict(self) -> Dict[str, Any]:
        """Serialização para salvamento"""
        return {
            'id': self.id,
            'name': self.name,
            'category': self.category.value,
            'symbol': self.symbol,
            'color': self.color,
            'weight': self.weight,
            'value': self.value,
            'rarity': self.rarity.value,
            'data': self.data,
            'stackable': self.stackable,
            'stack_size': self.stack_size,
            'current_stack': self.current_stack
        }
    
    @classmethod
    def from_dict(cls, data: Dict[str, Any]) -> 'Item':
        """Desserialização do salvamento"""
        item = cls(
            item_id=data['id'],
            name=data['name'],
            category=ItemCategory(data['category']),
            symbol=data['symbol'],
            color=data['color'],
            weight=data['weight'],
            value=data['value'],
            rarity=ItemRarity(data['rarity']),
            data=data.get('data', {})
        )
        item.stackable = data.get('stackable', False)
        item.stack_size = data.get('stack_size', 1)
        item.current_stack = data.get('current_stack', 1)
        return item
```

### 2. Sistema de Inventário
```python
class InventoryComponent(Component):
    """Componente para gerenciar inventário"""
    
    def __init__(self, owner: Entity, capacity: int = 10):
        super().__init__(owner)
        self.capacity = capacity
        self.items: List[Item] = []
        self.equipped = {
            'weapon': None,
            'armor': None,
            'accessory': None
        }
    
    def add_item(self, item: Item) -> bool:
        """Adiciona um item ao inventário"""
        if len(self.items) >= self.capacity:
            return False
        
        # Tenta empilhar com itens existentes
        if item.stackable:
            for existing in self.items:
                if existing.can_stack_with(item):
                    transferred = existing.merge_stack(item)
                    if transferred == item.current_stack:
                        return True
                    # Se não transferiu tudo, continua com o restante
        
        # Adiciona como novo item
        self.items.append(item)
        return True
    
    def remove_item(self, item_id: str, amount: int = 1) -> Optional[Item]:
        """Remove um item do inventário"""
        for i, item in enumerate(self.items):
            if item.id == item_id:
                if item.stackable and item.current_stack > amount:
                    # Remove apenas parte do stack
                    removed = item.split_stack(amount)
                    return removed
                else:
                    # Remove item completo
                    return self.items.pop(i)
        return None
    
    def has_item(self, item_id: str, amount: int = 1) -> bool:
        """Verifica se tem um item no inventário"""
        total = 0
        for item in self.items:
            if item.id == item_id:
                if item.stackable:
                    total += item.current_stack
                else:
                    total += 1
                
                if total >= amount:
                    return True
        return False
    
    def get_item(self, item_id: str) -> Optional[Item]:
        """Obtém um item pelo ID"""
        for item in self.items:
            if item.id == item_id:
                return item
        return None
    
    def get_items_by_category(self, category: ItemCategory) -> List[Item]:
        """Obtém todos os itens de uma categoria"""
        return [item for item in self.items if item.category == category]
    
    def equip_item(self, item: Item, slot: str) -> bool:
        """Equipa um item em um slot específico"""
        if slot not in self.equipped:
            return False
        
        # Verifica se o item está no inventário
        if item not in self.items:
            return False
        
        # Verifica categoria do item
        if (slot == 'weapon' and item.category != ItemCategory.WEAPON or
            slot == 'armor' and item.category != ItemCategory.ARMOR):
            return False
        
        # Desequipa item atual se houver
        self.unequip_slot(slot)
        
        # Equipa novo item
        self.equipped[slot] = item
        self.items.remove(item)
        
        # Aplica efeitos do item
        item.use(self.owner)
        
        return True
    
    def unequip_slot(self, slot: str) -> Optional[Item]:
        """Desequipa um slot"""
        if slot not in self.equipped or not self.equipped[slot]:
            return None
        
        item = self.equipped[slot]
        self.equipped[slot] = None
        
        # Adiciona de volta ao inventário
        self.add_item(item)
        
        # Remove efeitos do item
        # (Implementar lógica de remoção de bônus)
        
        return item
    
    def get_total_weight(self) -> float:
        """Calcula peso total do inventário"""
        total = 0.0
        for item in self.items:
            total += item.weight * (item.current_stack if item.stackable else 1)
        return total
    
    def is_overencumbered(self) -> bool:
        """Verifica se o inventário está sobrecarregado"""
        return self.get_total_weight() > self.capacity * 5  # 5 unidades de peso por slot
    
    def update(self, game_map: GameMap, event_system: EventSystem):
        """Atualiza o inventário"""
        pass  # Componente passivo
    
    def to_dict(self) -> Dict[str, Any]:
        data = super().to_dict()
        data.update({
            'capacity': self.capacity,
            'items': [item.to_dict() for item in self.items],
            'equipped': {
                slot: item.to_dict() if item else None
                for slot, item in self.equipped.items()
            }
        })
        return data
    
    @classmethod
    def from_dict(cls, data: Dict[str, Any], owner: Entity) -> 'InventoryComponent':
        comp = cls(owner, data['capacity'])
        comp.items = [Item.from_dict(item_data) for item_data in data['items']]
        
        # Carrega itens equipados
        for slot, item_data in data['equipped'].items():
            if item_data:
                comp.equipped[slot] = Item.from_dict(item_data)
        
        return comp
```

### 3. Container Component
```python
class ContainerComponent(Component):
    """Componente para recipientes (baús, etc.)"""
    
    def __init__(self, owner: Entity, capacity: int = 5, locked: bool = False):
        super().__init__(owner)
        self.capacity = capacity
        self.locked = locked
        self.items: List[Item] = []
        self.key_id: Optional[str] = None  # ID da chave que abre
        self.trap: Optional[Dict[str, Any]] = None
    
    def open(self, opener: Entity) -> Dict[str, Any]:
        """Abre o container"""
        if self.locked:
            if not self._try_unlock(opener):
                return {
                    'success': False,
                    'message': 'Container está trancado',
                    'locked': True
                }
        
        if self.trap:
            return self._trigger_trap(opener)
        
        return {
            'success': True,
            'message': 'Container aberto',
            'items': self.items.copy(),
            'container': self.owner
        }
    
    def _try_unlock(self, opener: Entity) -> bool:
        """Tenta destrancar o container"""
        if not self.key_id:
            return False  # Não pode ser destrancado
        
        inventory = opener.get_component('inventory')
        if not inventory:
            return False
        
        return inventory.has_item(self.key_id)
    
    def _trigger_trap(self, opener: Entity) -> Dict[str, Any]:
        """Ativa uma armadilha"""
        result = {'success': False, 'trapped': True}
        
        if self.trap['type'] == 'damage':
            health = opener.get_component('health')
            if health:
                damage = self.trap.get('damage', 10)
                health.take_damage(damage)
                result.update({
                    'damage': damage,
                    'message': f'Armadilha! {opener.name} sofreu {damage} de dano'
                })
        
        elif self.trap['type'] == 'poison':
            # Aplica efeito de veneno
            result['message'] = f'{opener.name} foi envenenado!'
        
        # Remove a armadilha após ser ativada
        self.trap = None
        
        return result
    
    def add_item(self, item: Item) -> bool:
        """Adiciona um item ao container"""
        if len(self.items) >= self.capacity:
            return False
        self.items.append(item)
        return True
    
    def take_item(self, item_id: str) -> Optional[Item]:
        """Pega um item do container"""
        for i, item in enumerate(self.items):
            if item.id == item_id:
                return self.items.pop(i)
        return None
    
    def update(self, game_map: GameMap, event_system: EventSystem):
        pass
    
    def to_dict(self) -> Dict[str, Any]:
        data = super().to_dict()
        data.update({
            'capacity': self.capacity,
            'locked': self.locked,
            'items': [item.to_dict() for item in self.items],
            'key_id': self.key_id,
            'trap': self.trap
        })
        return data
    
    @classmethod
    def from_dict(cls, data: Dict[str, Any], owner: Entity) -> 'ContainerComponent':
        comp = cls(owner, data['capacity'], data['locked'])
        comp.items = [Item.from_dict(item_data) for item_data in data['items']]
        comp.key_id = data.get('key_id')
        comp.trap = data.get('trap')
        return comp
```

---

## ⚔️ SISTEMAS DE COMBATE

### 1. Sistema de Turnos
```python
from typing import List, Optional
import heapq

class Turn:
    """Representa um turno no sistema de combate"""
    
    def __init__(self, entity: Entity, initiative: int):
        self.entity = entity
        self.initiative = initiative
        self.actions = []
        self.completed = False
    
    def __lt__(self, other: 'Turn') -> bool:
        # Para heap: menor initiative = maior prioridade
        return self.initiative > other.initiative

class CombatSystem:
    """Sistema de combate baseado em turnos"""
    
    def __init__(self):
        self.turn_queue: List[Turn] = []
        self.current_turn: Optional[Turn] = None
        self.round = 1
        self.active = False
    
    def start_combat(self, participants: List[Entity]):
        """Inicia um combate com os participantes"""
        self.active = True
        self.round = 1
        self.turn_queue = []
        
        # Calcula iniciativa para cada participante
        for entity in participants:
            initiative = self._calculate_initiative(entity)
            heapq.heappush(self.turn_queue, Turn(entity, initiative))
        
        # Inicia primeiro turno
        self._next_turn()
    
    def _calculate_initiative(self, entity: Entity) -> int:
        """Calcula iniciativa baseada em atributos"""
        base = 10
        
        # Bônus de agilidade (se existir no componente)
        combat = entity.get_component('combat')
        if combat:
            # Simples: entidades com maior defesa são mais lentas
            base -= min(combat.defense // 2, 5)
        
        # Adiciona aleatoriedade
        import random
        base += random.randint(1, 20)
        
        return base
    
    def _next_turn(self):
        """Avança para o próximo turno"""
        if not self.turn_queue:
            self._end_round()
            return
        
        self.current_turn = heapq.heappop(self.turn_queue)
        
        # Se a entidade não está mais viva, pula o turno
        health = self.current_turn.entity.get_component('health')
        if not health or not health.is_alive:
            self._next_turn()
            return
        
        print(f"Turno de {self.current_turn.entity.name}")
    
    def _end_round(self):
        """Finaliza o round atual e inicia o próximo"""
        self.round += 1
        
        # Verifica se o combate acabou
        active_entities = self._get_active_entities()
        if len(active_entities) <= 1:
            self.end_combat()
            return
        
        # Recalcula iniciativa para o próximo round
        for entity in active_entities:
            initiative = self._calculate_initiative(entity)
            heapq.heappush(self.turn_queue, Turn(entity, initiative))
        
        self._next_turn()
    
    def _get_active_entities(self) -> List[Entity]:
        """Retorna entidades ainda ativas no combate"""
        active = []
        all_entities = [turn.entity for turn in self.turn_queue]
        if self.current_turn:
            all_entities.append(self.current_turn.entity)
        
        for entity in all_entities:
            health = entity.get_component('health')
            if health and health.is_alive:
                active.append(entity)
        
        return active
    
    def execute_action(self, action_type: str, **kwargs) -> Dict[str, Any]:
        """Executa uma ação no turno atual"""
        if not self.current_turn or self.current_turn.completed:
            return {'success': False, 'message': 'Nenhum turno ativo'}
        
        entity = self.current_turn.entity
        
        if action_type == 'attack':
            target = kwargs.get('target')
            if not target:
                return {'success': False, 'message': 'Nenhum alvo especificado'}
            
            combat = entity.get_component('combat')
            if not combat:
                return {'success': False, 'message': 'Entidade não pode atacar'}
            
            result = combat.attack(target)
            self.current_turn.completed = True
            self._next_turn()
            return result
        
        elif action_type == 'use_item':
            item_id = kwargs.get('item_id')
            target = kwargs.get('target', entity)
            
            inventory = entity.get_component('inventory')
            if not inventory:
                return {'success': False, 'message': 'Sem inventário'}
            
            item = inventory.get_item(item_id)
            if not item:
                return {'success': False, 'message': 'Item não encontrado'}
            
            result = item.use(entity, target)
            if result.get('success'):
                # Remove item se for consumível
                if item.category == ItemCategory.CONSUMABLE:
                    inventory.remove_item(item_id)
            
            self.current_turn.completed = True
            self._next_turn()
            return result
        
        elif action_type == 'flee':
            # Tentativa de fuga
            import random
            success = random.random() > 0.5
            
            self.current_turn.completed = True
            self.current_turn = None
            
            if success:
                self.end_combat()
                return {'success': True, 'message': 'Fuga bem sucedida'}
            else:
                self._next_turn()
                return {'success': False, 'message': 'Falha na fuga'}
        
        self.current_turn.completed = True
        self._next_turn()
        return {'success': False, 'message': 'Ação desconhecida'}
    
    def end_combat(self):
        """Finaliza o combate"""
        self.active = False
        self.turn_queue.clear()
        self.current_turn = None
        print("Combate encerrado!")
    
    def update(self):
        """Atualiza o sistema de combate"""
        if not self.active:
            return
        
        # Se é turno de uma IA, executa automaticamente
        if (self.current_turn and 
            self.current_turn.entity.has_tag('enemy') and
            not self.current_turn.completed):
            
            ai = self.current_turn.entity.get_component('ai')
            if ai and ai.target:
                # Ataca o alvo
                self.execute_action('attack', target=ai.target)
```

### 2. Sistema de Experiência e Níveis
```python
class ExperienceComponent(Component):
    """Componente para gerenciar experiência e níveis"""
    
    def __init__(self, owner: Entity):
        super().__init__(owner)
        self.level = 1
        self.experience = 0
        self.experience_to_next = self._calculate_xp_needed()
        self.skill_points = 0
        self.attributes = {
            'strength': 10,
            'dexterity': 10,
            'constitution': 10,
            'intelligence': 10,
            'wisdom': 10,
            'charisma': 10
        }
        self.skills = {}  # Habilidades desbloqueadas
    
    def _calculate_xp_needed(self) -> int:
        """Calcula XP necessário para próximo nível"""
        # Fórmula: 100 * nível * 1.5
        return int(100 * self.level * 1.5)
    
    def add_experience(self, amount: int) -> bool:
        """Adiciona experiência, retorna True se subiu de nível"""
        self.experience += amount
        
        leveled_up = False
        while self.experience >= self.experience_to_next:
            self._level_up()
            leveled_up = True
        
        return leveled_up
    
    def _level_up(self):
        """Aumenta o nível do personagem"""
        self.experience -= self.experience_to_next
        self.level += 1
        self.skill_points += 5  # Pontos para distribuir
        self.experience_to_next = self._calculate_xp_needed()
        
        # Melhora atributos base
        self.attributes['constitution'] += 1
        
        # Recupera vida total
        health = self.owner.get_component('health')
        if health:
            health.max_hp += 10
            health.heal(health.max_hp)  # Cura totalmente
        
        print(f"{self.owner.name} subiu para o nível {self.level}!")
    
    def improve_attribute(self, attribute: str) -> bool:
        """Melhora um atributo usando skill points"""
        if attribute not in self.attributes:
            return False
        
        if self.skill_points < 1:
            return False
        
        self.attributes[attribute] += 1
        self.skill_points -= 1
        
        # Aplica efeitos baseados no atributo
        if attribute == 'strength':
            combat = self.owner.get_component('combat')
            if combat:
                combat.attack_power += 2
        
        elif attribute == 'constitution':
            health = self.owner.get_component('health')
            if health:
                health.max_hp += 5
                health.current_hp += 5
        
        return True
    
    def get_xp_from_enemy(self, enemy: Entity) -> int:
        """Calcula XP ganho ao derrotar um inimigo"""
        enemy_level = 1
        exp_component = enemy.get_component('experience')
        if exp_component:
            enemy_level = exp_component.level
        
        # Fórmula base: 25 * nível_inimigo
        base_xp = 25 * enemy_level
        
        # Modificador baseado na diferença de nível
        level_diff = enemy_level - self.level
        if level_diff > 0:
            # Inimigo mais forte dá mais XP
            base_xp *= (1 + level_diff * 0.2)
        elif level_diff < 0:
            # Inimigo mais fraco dá menos XP
            base_xp *= max(0.5, 1 + level_diff * 0.1)
        
        return int(base_xp)
    
    def update(self, game_map: GameMap, event_system: EventSystem):
        pass
    
    def to_dict(self) -> Dict[str, Any]:
        data = super().to_dict()
        data.update({
            'level': self.level,
            'experience': self.experience,
            'experience_to_next': self.experience_to_next,
            'skill_points': self.skill_points,
            'attributes': self.attributes,
            'skills': self.skills
        })
        return data
    
    @classmethod
    def from_dict(cls, data: Dict[str, Any], owner: Entity) -> 'ExperienceComponent':
        comp = cls(owner)
        comp.level = data['level']
        comp.experience = data['experience']
        comp.experience_to_next = data['experience_to_next']
        comp.skill_points = data['skill_points']
        comp.attributes = data['attributes']
        comp.skills = data.get('skills', {})
        return comp
```

---

## 📜 QUEST E PROGRESSÃO

### 1. Sistema de Missões
```python
from typing import List, Dict, Any, Optional
from enum import Enum
from datetime import datetime

class QuestState(Enum):
    """Estados de uma missão"""
    NOT_STARTED = "not_started"
    IN_PROGRESS = "in_progress"
    COMPLETED = "completed"
    FAILED = "failed"

class QuestType(Enum):
    """Tipos de missões"""
    KILL = "kill"
    COLLECT = "collect"
    ESCORT = "escort"
    DELIVER = "deliver"
    DISCOVER = "discover"
    CRAFT = "craft"

class QuestObjective:
    """Objetivo de uma missão"""
    
    def __init__(self,
                 objective_type: QuestType,
                 target: str,  # ID do alvo
                 required_amount: int = 1,
                 current_amount: int = 0,
                 description: str = ""):
        
        self.objective_type = objective_type
        self.target = target
        self.required_amount = required_amount
        self.current_amount = current_amount
        self.description = description
        self.completed = False
    
    def update_progress(self, amount: int = 1) -> bool:
        """Atualiza progresso do objetivo"""
        if self.completed:
            return True
        
        self.current_amount = min(self.required_amount, self.current_amount + amount)
        
        if self.current_amount >= self.required_amount:
            self.completed = True
            return True
        
        return False
    
    def check_completion(self) -> bool:
        """Verifica se o objetivo foi completado"""
        self.completed = self.current_amount >= self.required_amount
        return self.completed
    
    def to_dict(self) -> Dict[str, Any]:
        return {
            'objective_type': self.objective_type.value,
            'target': self.target,
            'required_amount': self.required_amount,
            'current_amount': self.current_amount,
            'description': self.description,
            'completed': self.completed
        }
    
    @classmethod
    def from_dict(cls, data: Dict[str, Any]) -> 'QuestObjective':
        obj = cls(
            objective_type=QuestType(data['objective_type']),
            target=data['target'],
            required_amount=data['required_amount'],
            current_amount=data['current_amount'],
            description=data['description']
        )
        obj.completed = data['completed']
        return obj

class QuestReward:
    """Recompensa de uma missão"""
    
    def __init__(self,
                 experience: int = 0,
                 gold: int = 0,
                 items: List[Dict[str, Any]] = None,
                 reputation: Dict[str, int] = None):
        
        self.experience = experience
        self.gold = gold
        self.items = items or []
        self.reputation = reputation or {}
    
    def apply(self, player: Entity):
        """Aplica a recompensa ao jogador"""
        # Experiência
        exp_component = player.get_component('experience')
        if exp_component:
            exp_component.add_experience(self.experience)
        
        # Ouro (precisa de componente de inventário com gold)
        inventory = player.get_component('inventory')
        if inventory and self.gold > 0:
            # Adiciona ouro ao inventário
            gold_item = Item(
                item_id="gold",
                name="Gold",
                category=ItemCategory.MISC,
                symbol="$",
                color="yellow",
                weight=0,
                value=self.gold
            )
            gold_item.stackable = True
            gold_item.stack_size = 1000
            gold_item.current_stack = self.gold
            inventory.add_item(gold_item)
        
        # Itens
        for item_data in self.items:
            item = ItemFactory.create_item(item_data)
            if item:
                inventory.add_item(item) if inventory else None
    
    def to_dict(self) -> Dict[str, Any]:
        return {
            'experience': self.experience,
            'gold': self.gold,
            'items': self.items,
            'reputation': self.reputation
        }
    
    @classmethod
    def from_dict(cls, data: Dict[str, Any]) -> 'QuestReward':
        return cls(
            experience=data['experience'],
            gold=data['gold'],
            items=data.get('items', []),
            reputation=data.get('reputation', {})
        )

class Quest:
    """Representa uma missão no jogo"""
    
    def __init__(self,
                 quest_id: str,
                 name: str,
                 description: str,
                 giver: str,  # ID do NPC que dá a missão
                 level_requirement: int = 1):
        
        self.id = quest_id
        self.name = name
        self.description = description
        self.giver = giver
        self.level_requirement = level_requirement
        
        self.state = QuestState.NOT_STARTED
        self.objectives: List[QuestObjective] = []
        self.rewards = QuestReward()
        
        self.start_time: Optional[datetime] = None
        self.completion_time: Optional[datetime] = None
        self.time_limit: Optional[int] = None  # Em minutos
    
    def start(self) -> bool:
        """Inicia a missão"""
        if self.state != QuestState.NOT_STARTED:
            return False
        
        self.state = QuestState.IN_PROGRESS
        self.start_time = datetime.now()
        return True
    
    def complete(self) -> bool:
        """Completa a missão"""
        if self.state != QuestState.IN_PROGRESS:
            return False
        
        # Verifica se todos objetivos estão completos
        if not all(obj.completed for obj in self.objectives):
            return False
        
        self.state = QuestState.COMPLETED
        self.completion_time = datetime.now()
        return True
    
    def fail(self):
        """Falha a missão"""
        self.state = QuestState.FAILED
    
    def check_time_limit(self) -> bool:
        """Verifica se a missão expirou"""
        if not self.time_limit or not self.start_time:
            return False
        
        elapsed = (datetime.now() - self.start_time).total_seconds() / 60
        if elapsed > self.time_limit:
            self.fail()
            return True
        
        return False
    
    def update_objective(self, 
                        objective_type: QuestType,
                        target: str,
                        amount: int = 1) -> bool:
        """Atualiza progresso de um objetivo"""
        for objective in self.objectives:
            if (objective.objective_type == objective_type and 
                objective.target == target and
                not objective.completed):
                
                objective.update_progress(amount)
                
                # Verifica se a missão foi completada
                if all(obj.completed for obj in self.objectives):
                    self.complete()
                
                return True
        
        return False
    
    def add_objective(self, objective: QuestObjective):
        """Adiciona um objetivo à missão"""
        self.objectives.append(objective)
    
    def set_rewards(self, reward: QuestReward):
        """Define as recompensas da missão"""
        self.rewards = reward
    
    def get_progress(self) -> Dict[str, Any]:
        """Retorna progresso da missão"""
        total = len(self.objectives)
        completed = sum(1 for obj in self.objectives if obj.completed)
        
        return {
            'total': total,
            'completed': completed,
            'percentage': (completed / total * 100) if total > 0 else 0
        }
    
    def to_dict(self) -> Dict[str, Any]:
        return {
            'id': self.id,
            'name': self.name,
            'description': self.description,
            'giver': self.giver,
            'level_requirement': self.level_requirement,
            'state': self.state.value,
            'objectives': [obj.to_dict() for obj in self.objectives],
            'rewards': self.rewards.to_dict(),
            'start_time': self.start_time.isoformat() if self.start_time else None,
            'completion_time': self.completion_time.isoformat() if self.completion_time else None,
            'time_limit': self.time_limit
        }
    
    @classmethod
    def from_dict(cls, data: Dict[str, Any]) -> 'Quest':
        quest = cls(
            quest_id=data['id'],
            name=data['name'],
            description=data['description'],
            giver=data['giver'],
            level_requirement=data['level_requirement']
        )
        
        quest.state = QuestState(data['state'])
        quest.objectives = [QuestObjective.from_dict(obj_data) 
                           for obj_data in data['objectives']]
        quest.rewards = QuestReward.from_dict(data['rewards'])
        
        if data.get('start_time'):
            quest.start_time = datetime.fromisoformat(data['start_time'])
        if data.get('completion_time'):
            quest.completion_time = datetime.fromisoformat(data['completion_time'])
        
        quest.time_limit = data.get('time_limit')
        
        return quest
```

### 2. Quest Manager
```python
class QuestManager:
    """Gerencia todas as missões do jogo"""
    
    def __init__(self):
        self.active_quests: Dict[str, Quest] = {}  # Quest ID -> Quest
        self.completed_quests: Dict[str, Quest] = {}
        self.failed_quests: Dict[str, Quest] = {}
        self.available_quests: Dict[str, Quest] = {}  # Missões disponíveis
    
    def accept_quest(self, quest_id: str) -> bool:
        """Aceita uma missão disponível"""
        if quest_id not in self.available_quests:
            return False
        
        quest = self.available_quests.pop(quest_id)
        if quest.start():
            self.active_quests[quest_id] = quest
            return True
        
        return False
    
    def complete_quest(self, quest_id: str, player: Entity) -> bool:
        """Completa uma missão ativa"""
        if quest_id not in self.active_quests:
            return False
        
        quest = self.active_quests[quest_id]
        if quest.complete():
            # Aplica recompensas
            quest.rewards.apply(player)
            
            # Move para completadas
            del self.active_quests[quest_id]
            self.completed_quests[quest_id] = quest
            
            return True
        
        return False
    
    def fail_quest(self, quest_id: str):
        """Falha uma missão"""
        if quest_id in self.active_quests:
            quest = self.active_quests.pop(quest_id)
            quest.fail()
            self.failed_quests[quest_id] = quest
    
    def update_quests(self, event_system: EventSystem):
        """Atualiza todas as missões baseadas em eventos"""
        # Processa eventos relacionados a missões
        # Esta função seria chamada quando eventos ocorrem
    
    def check_quest_progress(self, 
                           objective_type: QuestType,
                           target: str,
                           amount: int = 1):
        """Verifica e atualiza progresso das missões"""
        for quest in list(self.active_quests.values()):
            quest.update_objective(objective_type, target, amount)
    
    def get_active_quests(self) -> List[Quest]:
        """Retorna missões ativas"""
        return list(self.active_quests.values())
    
    def get_quest_status(self, quest_id: str) -> Optional[Dict[str, Any]]:
        """Retorna status de uma missão específica"""
        if quest_id in self.active_quests:
            quest = self.active_quests[quest_id]
            return {
                'state': quest.state,
                'progress': quest.get_progress(),
                'objectives': [
                    {
                        'description': obj.description,
                        'progress': f"{obj.current_amount}/{obj.required_amount}",
                        'completed': obj.completed
                    }
                    for obj in quest.objectives
                ]
            }
        
        return None
    
    def add_available_quest(self, quest: Quest):
        """Adiciona uma missão disponível"""
        self.available_quests[quest.id] = quest
    
    def to_dict(self) -> Dict[str, Any]:
        return {
            'active_quests': {qid: q.to_dict() for qid, q in self.active_quests.items()},
            'completed_quests': {qid: q.to_dict() for qid, q in self.completed_quests.items()},
            'failed_quests': {qid: q.to_dict() for qid, q in self.failed_quests.items()},
            'available_quests': {qid: q.to_dict() for qid, q in self.available_quests.items()}
        }
    
    @classmethod
    def from_dict(cls, data: Dict[str, Any]) -> 'QuestManager':
        manager = cls()
        
        for quest_id, quest_data in data.get('active_quests', {}).items():
            manager.active_quests[quest_id] = Quest.from_dict(quest_data)
        
        for quest_id, quest_data in data.get('completed_quests', {}).items():
            manager.completed_quests[quest_id] = Quest.from_dict(quest_data)
        
        for quest_id, quest_data in data.get('failed_quests', {}).items():
            manager.failed_quests[quest_id] = Quest.from_dict(quest_data)
        
        for quest_id, quest_data in data.get('available_quests', {}).items():
            manager.available_quests[quest_id] = Quest.from_dict(quest_data)
        
        return manager
```

---

## 💾 SALVAMENTO DE JOGO

### 1. Sistema de Salvamento
```python
import json
import pickle
import zlib
from datetime import datetime
from pathlib import Path
from typing import Dict, Any

class SaveGame:
    """Representa um save do jogo"""
    
    def __init__(self, 
                 save_id: str,
                 timestamp: datetime,
                 game_data: Dict[str, Any]):
        
        self.save_id = save_id
        self.timestamp = timestamp
        self.game_data = game_data
        self.description = ""
    
    def to_dict(self) -> Dict[str, Any]:
        return {
            'save_id': self.save_id,
            'timestamp': self.timestamp.isoformat(),
            'description': self.description,
            'game_data': self.game_data
        }
    
    @classmethod
    def from_dict(cls, data: Dict[str, Any]) -> 'SaveGame':
        save = cls(
            save_id=data['save_id'],
            timestamp=datetime.fromisoformat(data['timestamp']),
            game_data=data['game_data']
        )
        save.description = data.get('description', '')
        return save

class SaveSystem:
    """Sistema de gerenciamento de saves"""
    
    def __init__(self, save_dir: str = "saves"):
        self.save_dir = Path(save_dir)
        self.save_dir.mkdir(exist_ok=True)
        self.current_game_data: Dict[str, Any] = {}
    
    def create_save(self, 
                    game_state: Dict[str, Any],
                    description: str = "") -> str:
        """Cria um novo save"""
        from uuid import uuid4
        
        save_id = f"save_{uuid4().hex[:8]}"
        timestamp = datetime.now()
        
        save = SaveGame(
            save_id=save_id,
            timestamp=timestamp,
            game_data=game_state
        )
        save.description = description
        
        # Salva em arquivo
        self._write_save(save)
        
        return save_id
    
    def load_save(self, save_id: str) -> Optional[Dict[str, Any]]:
        """Carrega um save específico"""
        save_file = self.save_dir / f"{save_id}.json"
        
        if not save_file.exists():
            return None
        
        try:
            with open(save_file, 'r', encoding='utf-8') as f:
                data = json.load(f)
            
            save = SaveGame.from_dict(data)
            self.current_game_data = save.game_data
            return save.game_data
        
        except Exception as e:
            print(f"Erro ao carregar save: {e}")
            return None
    
    def delete_save(self, save_id: str) -> bool:
        """Deleta um save"""
        save_file = self.save_dir / f"{save_id}.json"
        
        if save_file.exists():
            save_file.unlink()
            return True
        
        return False
    
    def list_saves(self) -> List[Dict[str, Any]]:
        """Lista todos os saves disponíveis"""
        saves = []
        
        for save_file in self.save_dir.glob("*.json"):
            try:
                with open(save_file, 'r', encoding='utf-8') as f:
                    data = json.load(f)
                
                save = SaveGame.from_dict(data)
                saves.append({
                    'id': save.save_id,
                    'timestamp': save.timestamp,
                    'description': save.description,
                    'filename': save_file.name
                })
            
            except Exception as e:
                print(f"Erro ao ler save {save_file}: {e}")
        
        # Ordena por timestamp (mais recente primeiro)
        saves.sort(key=lambda x: x['timestamp'], reverse=True)
        
        return saves
    
    def get_autosave_data(self) -> Dict[str, Any]:
        """Prepara dados para autosave"""
        # Esta função deve coletar todos os dados necessários do jogo
        return {
            'timestamp': datetime.now().isoformat(),
            'game_data': self.current_game_data
        }
    
    def create_autosave(self):
        """Cria um autosave"""
        autosave_id = "autosave"
        autosave_file = self.save_dir / f"{autosave_id}.json"
        
        save = SaveGame(
            save_id=autosave_id,
            timestamp=datetime.now(),
            game_data=self.current_game_data
        )
        save.description = "Autosave"
        
        self._write_save(save, autosave_file)
    
    def _write_save(self, save: SaveGame, filepath: Optional[Path] = None):
        """Escreve save no disco"""
        if filepath is None:
            filepath = self.save_dir / f"{save.save_id}.json"
        
        try:
            # Converte objetos complexos para dicionários
            serializable_data = self._make_serializable(save.to_dict())
            
            with open(filepath, 'w', encoding='utf-8') as f:
                json.dump(serializable_data, f, indent=2, ensure_ascii=False)
        
        except Exception as e:
            print(f"Erro ao salvar jogo: {e}")
    
    def _make_serializable(self, obj: Any) -> Any:
        """Converte objetos não-serializáveis para formatos serializáveis"""
        if isinstance(obj, (str, int, float, bool, type(None))):
            return obj
        
        elif isinstance(obj, (list, tuple)):
            return [self._make_serializable(item) for item in obj]
        
        elif isinstance(obj, dict):
            return {key: self._make_serializable(value) 
                   for key, value in obj.items()}
        
        elif hasattr(obj, 'to_dict'):
            return self._make_serializable(obj.to_dict())
        
        else:
            # Tenta converter para string
            return str(obj)
    
    def compress_save(self, save_id: str) -> bool:
        """Comprime um save para economizar espaço"""
        save_file = self.save_dir / f"{save_id}.json"
        compressed_file = self.save_dir / f"{save_id}.json.gz"
        
        if not save_file.exists():
            return False
        
        try:
            with open(save_file, 'rb') as f:
                data = f.read()
            
            compressed = zlib.compress(data)
            
            with open(compressed_file, 'wb') as f:
                f.write(compressed)
            
            # Opcional: remove arquivo original
            # save_file.unlink()
            
            return True
        
        except Exception as e:
            print(f"Erro ao comprimir save: {e}")
            return False
    
    def backup_all_saves(self, backup_dir: str = "backups"):
        """Cria backup de todos os saves"""
        backup_path = Path(backup_dir)
        backup_path.mkdir(exist_ok=True)
        
        timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")
        backup_file = backup_path / f"saves_backup_{timestamp}.zip"
        
        import zipfile
        
        try:
            with zipfile.ZipFile(backup_file, 'w', zipfile.ZIP_DEFLATED) as zipf:
                for save_file in self.save_dir.glob("*.json"):
                    zipf.write(save_file, save_file.name)
            
            print(f"Backup criado: {backup_file}")
            return True
        
        except Exception as e:
            print(f"Erro ao criar backup: {e}")
            return False
```

### 2. Game State Manager
```python
class GameState:
    """Gerencia o estado completo do jogo"""
    
    def __init__(self):
        self.player: Optional[Entity] = None
        self.current_map: Optional[GameMap] = None
        self.maps: Dict[int, GameMap] = {}  # level -> GameMap
        self.quest_manager = QuestManager()
        self.event_system = EventSystem()
        self.combat_system = CombatSystem()
        self.game_time = 0  # Tempo do jogo em turns
        self.game_mode = "explore"  # explore, combat, menu, dialog
        
        # Configurações
        self.settings = {
            'difficulty': 'normal',
            'autosave': True,
            'autosave_interval': 100,  # turns
            'last_autosave': 0
        }
    
    def initialize_new_game(self, player_name: str):
        """Inicializa um novo jogo"""
        # Cria jogador
        self.player = EntityFactory.create_player(
            player_name,
            Vector2D(0, 0)
        )
        
        # Cria mapa inicial
        self.current_map = GameMap(80, 50, level=1)
        self.current_map.generate_dungeon()
        
        # Posiciona jogador
        if self.current_map.player_start:
            self.player.position = self.current_map.player_start
            self.current_map.add_entity(self.player)
        
        # Adiciona alguns inimigos
        self._populate_map(self.current_map)
        
        # Adiciona missão inicial
        self._create_initial_quest()
        
        self.game_time = 0
        self.game_mode = "explore"
    
    def _populate_map(self, game_map: GameMap, enemy_count: int = 10):
        """Popula o mapa com inimigos e itens"""
        import random
        
        for _ in range(enemy_count):
            # Encontra posição aleatória walkable
            while True:
                x = random.randint(1, game_map.width - 2)
                y = random.randint(1, game_map.height - 2)
                
                if (game_map.is_walkable(x, y) and 
                    not game_map.get_entity_at(x, y)):
                    
                    # Cria inimigo
                    enemy_type = random.choice(['goblin', 'orc', 'skeleton'])
                    
                    if enemy_type == 'goblin':
                        enemy = EntityFactory.create_goblin(Vector2D(x, y))
                    # Adicionar outros tipos...
                    
                    game_map.add_entity(enemy)
                    break
        
        # Adiciona alguns itens
        for _ in range(5):
            while True:
                x = random.randint(1, game_map.width - 2)
                y = random.randint(1, game_map.height - 2)
                
                if (game_map.is_walkable(x, y) and 
                    not game_map.get_entity_at(x, y)):
                    
                    item = EntityFactory.create_healing_potion(Vector2D(x, y))
                    game_map.add_entity(item)
                    break
    
    def _create_initial_quest(self):
        """Cria missão inicial"""
        quest = Quest(
            quest_id="first_quest",
            name="Primeira Missão",
            description="Derrote 3 goblins para provar seu valor.",
            giver="town_elder",
            level_requirement=1
        )
        
        # Objetivo: matar 3 goblins
        objective = QuestObjective(
            objective_type=QuestType.KILL,
            target="goblin",
            required_amount=3,
            description="Derrote 3 Goblins"
        )
        quest.add_objective(objective)
        
        # Recompensa
        reward = QuestReward(
            experience=100,
            gold=50,
            items=[{'id': 'healing_potion', 'amount': 3}]
        )
        quest.set_rewards(reward)
        
        # Adiciona ao gerenciador
        self.quest_manager.add_available_quest(quest)
        self.quest_manager.accept_quest("first_quest")
    
    def update(self):
        """Atualiza o estado do jogo"""
        self.game_time += 1
        
        # Atualiza sistemas
        self.event_system.process_queue()
        self.combat_system.update()
        
        # Atualiza IA de entidades
        for entity in self.current_map.entities.values():
            if entity.has_component('ai'):
                ai = entity.get_component('ai')
                ai.update(self.current_map, self.event_system)
        
        # Autosave
        if (self.settings['autosave'] and 
            self.game_time - self.settings['last_autosave'] >= 
            self.settings['autosave_interval']):
            
            self.settings['last_autosave'] = self.game_time
            # Salva o jogo
    
    def change_map(self, new_level: int) -> bool:
        """Muda para um mapa diferente"""
        if new_level in self.maps:
            self.current_map = self.maps[new_level]
        else:
            # Gera novo mapa
            new_map = GameMap(80, 50, level=new_level)
            new_map.generate_dungeon()
            self.maps[new_level] = new_map
            self.current_map = new_map
        
        # Reposiciona jogador
        if self.player:
            if new_level > self.current_map.level:
                # Indo para baixo
                if self.current_map.stairs_down:
                    self.player.position = self.current_map.stairs_down
            else:
                # Indo para cima
                if self.current_map.stairs_up:
                    self.player.position = self.current_map.stairs_up
            
            self.current_map.add_entity(self.player)
        
        return True
    
    def save_game(self) -> Dict[str, Any]:
        """Prepara dados para salvamento"""
        return {
            'player': self.player.to_dict() if self.player else None,
            'current_map': self.current_map.to_dict() if self.current_map else None,
            'maps': {level: game_map.to_dict() 
                    for level, game_map in self.maps.items()},
            'quest_manager': self.quest_manager.to_dict(),
            'game_time': self.game_time,
            'game_mode': self.game_mode,
            'settings': self.settings,
            'version': '1.0.0'
        }
    
    def load_game(self, data: Dict[str, Any]):
        """Carrega dados do salvamento"""
        # Carrega jogador
        if data['player']:
            self.player = Entity.from_dict(data['player'])
        
        # Carrega mapas
        self.maps = {}
        for level, map_data in data.get('maps', {}).items():
            self.maps[int(level)] = GameMap.from_dict(map_data)
        
        # Carrega mapa atual
        if data['current_map']:
            self.current_map = GameMap.from_dict(data['current_map'])
        
        # Carrega quest manager
        self.quest_manager = QuestManager.from_dict(data['quest_manager'])
        
        # Outros dados
        self.game_time = data['game_time']
        self.game_mode = data['game_mode']
        self.settings = data['settings']
```

---

## 🤖 IA E PATHFINDING

### 1. Algoritmo A* para Pathfinding
```python
from typing import List, Tuple, Dict, Optional
import heapq

class Node:
    """Nó para o algoritmo A*"""
    
    def __init__(self, position: Tuple[int, int], parent: Optional['Node'] = None):
        self.position = position
        self.parent = parent
        self.g = 0  # Custo do início até este nó
        self.h = 0  # Heurística até o fim
        self.f = 0  # g + h
    
    def __eq__(self, other: 'Node') -> bool:
        return self.position == other.position
    
    def __lt__(self, other: 'Node') -> bool:
        return self.f < other.f

def a_star(game_map: GameMap, 
           start: Tuple[int, int], 
           goal: Tuple[int, int],
           diagonal_movement: bool = False) -> List[Tuple[int, int]]:
    """
    Implementação do algoritmo A* para pathfinding.
    Retorna uma lista de posições do caminho encontrado.
    """
    
    # Função heurística (distância Manhattan)
    def heuristic(a: Tuple[int, int], b: Tuple[int, int]) -> int:
        return abs(a[0] - b[0]) + abs(a[1] - b[1])
    
    # Cria nós inicial e final
    start_node = Node(start)
    goal_node = Node(goal)
    
    # Listas aberta e fechada
    open_list = []
    closed_list = []
    
    # Adiciona nó inicial à lista aberta
    heapq.heappush(open_list, start_node)
    
    # Direções de movimento
    if diagonal_movement:
        directions = [(-1, 0), (1, 0), (0, -1), (0, 1),
                     (-1, -1), (-1, 1), (1, -1), (1, 1)]
    else:
        directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]
    
    while open_list:
        # Pega o nó com menor f
        current_node = heapq.heappop(open_list)
        closed_list.append(current_node)
        
        # Verifica se chegou ao objetivo
        if current_node == goal_node:
            path = []
            current = current_node
            while current is not None:
                path.append(current.position)
                current = current.parent
            return path[::-1]  # Retorna caminho invertido
        
        # Gera nós filhos
        children = []
        for direction in directions:
            node_position = (
                current_node.position[0] + direction[0],
                current_node.position[1] + direction[1]
            )
            
            # Verifica se está dentro do mapa
            if not game_map.is_in_bounds(*node_position):
                continue
            
            # Verifica se é walkable
            if not game_map.is_walkable(*node_position):
                continue
            
            # Cria novo nó
            new_node = Node(node_position, current_node)
            children.append(new_node)
        
        # Processa cada filho
        for child in children:
            # Verifica se já está na lista fechada
            if any(closed_child == child for closed_child in closed_list):
                continue
            
            # Calcula custos
            child.g = current_node.g + 1
            child.h = heuristic(child.position, goal_node.position)
            child.f = child.g + child.h
            
            # Verifica se já está na lista aberta com menor g
            open_node = next((open_node for open_node in open_list 
                            if open_node == child and open_node.g <= child.g), None)
            
            if open_node is not None:
                continue
            
            # Adiciona à lista aberta
            heapq.heappush(open_list, child)
    
    # Se chegou aqui, não encontrou caminho
    return []

def bfs_pathfinding(game_map: GameMap, 
                    start: Tuple[int, int], 
                    goal: Tuple[int, int]) -> List[Tuple[int, int]]:
    """
    Pathfinding usando BFS (Breadth-First Search).
    Mais simples que A*, mas garante caminho mais curto em grids uniformes.
    """
    from collections import deque
    
    # Verifica se o objetivo é alcançável
    if not game_map.is_walkable(*goal):
        return []
    
    # Fila para BFS
    queue = deque([start])
    
    # Dicionário para rastrear caminho
    came_from = {start: None}
    
    # Direções
    directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]
    
    while queue:
        current = queue.popleft()
        
        # Verifica se chegou ao objetivo
        if current == goal:
            # Reconstrói caminho
            path = []
            while current is not None:
                path.append(current)
                current = came_from[current]
            return path[::-1]
        
        # Explora vizinhos
        for direction in directions:
            neighbor = (current[0] + direction[0], current[1] + direction[1])
            
            # Verifica se está dentro do mapa
            if not game_map.is_in_bounds(*neighbor):
                continue
            
            # Verifica se é walkable e não foi visitado
            if (game_map.is_walkable(*neighbor) and 
                neighbor not in came_from):
                
                came_from[neighbor] = current
                queue.append(neighbor)
    
    # Se chegou aqui, não encontrou caminho
    return []

def find_nearest_entity(game_map: GameMap, 
                       start_pos: Tuple[int, int],
                       entity_type: EntityType) -> Optional[Tuple[int, int]]:
    """
    Encontra a entidade mais próxima de um tipo específico.
    Usa BFS para encontrar o caminho mais curto.
    """
    from collections import deque
    
    queue = deque([start_pos])
    visited = {start_pos}
    
    directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]
    
    while queue:
        current = queue.popleft()
        
        # Verifica se há entidade do tipo desejado nesta posição
        entity = game_map.get_entity_at(*current)
        if entity and entity.type == entity_type:
            return current
        
        # Explora vizinhos
        for direction in directions:
            neighbor = (current[0] + direction[0], current[1] + direction[1])
            
            if (neighbor not in visited and 
                game_map.is_in_bounds(*neighbor) and 
                game_map.is_walkable(*neighbor)):
                
                visited.add(neighbor)
                queue.append(neighbor)
    
    return None
```

### 2. Sistema de Campo de Visão (FOV)
```python
def calculate_fov(game_map: GameMap, 
                 center_x: int, 
                 center_y: int, 
                 radius: int = 8) -> List[List[bool]]:
    """
    Calcula campo de visão usando algoritmo shadow casting.
    Retorna uma matriz booleana indicando tiles visíveis.
    """
    
    # Inicializa matriz de visibilidade
    fov_map = [[False for _ in range(game_map.width)] 
               for _ in range(game_map.height)]
    
    # Marca o centro como visível
    fov_map[center_y][center_x] = True
    
    # Função para verificar se um tile bloqueia visão
    def blocks_light(x: int, y: int) -> bool:
        if not game_map.is_in_bounds(x, y):
            return True
        tile = game_map.get_tile(x, y)
        return tile is None or not tile.transparent
    
    # Função para marcar tile como visível
    def set_visible(x: int, y: int):
        if game_map.is_in_bounds(x, y):
            fov_map[y][x] = True
    
    # Shadow casting em todas as direções
    for octant in range(8):
        _cast_light(center_x, center_y, 1, 1.0, 0.0, radius, 
                   octant, set_visible, blocks_light)
    
    return fov_map

def _cast_light(cx: int, cy: int, row: int, 
                start_slope: float, end_slope: float, 
                radius: int, octant: int,
                set_visible: callable, blocks_light: callable):
    """
    Função recursiva para shadow casting.
    Baseada no algoritmo descrito em: http://www.roguebasin.com/index.php?title=FOV_using_recursive_shadowcasting
    """
    
    if start_slope < end_slope:
        return
    
    radius_squared = radius * radius
    
    for j in range(row, radius + 1):
        blocked = False
        for i in range(j, -1, -1):
            # Coordenadas no octante atual
            x, y = _transform_octant(i, j, cx, cy, octant)
            
            # Calcula slopes
            l_slope = (i - 0.5) / (j + 0.5)
            r_slope = (i + 0.5) / (j - 0.5)
            
            if start_slope < r_slope:
                continue
            elif end_slope > l_slope:
                break
            
            # Verifica se está dentro do raio
            if i * i + j * j < radius_squared:
                set_visible(x, y)
            
            # Verifica se bloqueia luz
            if blocked:
                if blocks_light(x, y):
                    new_start_slope = r_slope
                    continue
                else:
                    blocked = False
                    start_slope = new_start_slope
            else:
                if blocks_light(x, y) and j < radius:
                    blocked = True
                    new_start_slope = r_slope
                    _cast_light(cx, cy, j + 1, start_slope, l_slope, 
                               radius, octant, set_visible, blocks_light)
        
        if blocked:
            break

def _transform_octant(x: int, y: int, cx: int, cy: int, octant: int) -> Tuple[int, int]:
    """Transforma coordenadas do octante para coordenadas do mapa."""
    if octant == 0: return (cx + x, cy - y)
    if octant == 1: return (cx + y, cy - x)
    if octant == 2: return (cx + y, cy + x)
    if octant == 3: return (cx + x, cy + y)
    if octant == 4: return (cx - x, cy + y)
    if octant == 5: return (cx - y, cy + x)
    if octant == 6: return (cx - y, cy - x)
    if octant == 7: return (cx - x, cy - y)
    return (cx, cy)

def update_fov(game_map: GameMap, player: Entity):
    """Atualiza campo de visão do jogador e descoberta de tiles."""
    if not player:
        return
    
    # Calcula FOV
    fov_map = calculate_fov(game_map, 
                           player.position.x, 
                           player.position.y, 
                           radius=8)
    
    # Atualiza tiles
    for y in range(game_map.height):
        for x in range(game_map.width):
            tile = game_map.get_tile(x, y)
            if tile:
                if fov_map[y][x]:
                    tile.visible = True
                    tile.discovered = True
                else:
                    tile.visible = False
```

---

## 🎨 EFEITOS VISUAIS NO TERMINAL

### 1. Sistema de Renderização
```python
import os
import sys
import time
from typing import List, Optional

class TerminalRenderer:
    """Sistema de renderização para terminal"""
    
    # Cores ANSI
    COLORS = {
        'black': '\033[30m',
        'red': '\033[31m',
        'green': '\033[32m',
        'yellow': '\033[33m',
        'blue': '\033[34m',
        'magenta': '\033[35m',
        'cyan': '\033[36m',
        'white': '\033[37m',
        'bright_black': '\033[90m',
        'bright_red': '\033[91m',
        'bright_green': '\033[92m',
        'bright_yellow': '\033[93m',
        'bright_blue': '\033[94m',
        'bright_magenta': '\033[95m',
        'bright_cyan': '\033[96m',
        'bright_white': '\033[97m',
        'reset': '\033[0m'
    }
    
    # Estilos ANSI
    STYLES = {
        'bold': '\033[1m',
        'dim': '\033[2m',
        'italic': '\033[3m',
        'underline': '\033[4m',
        'blink': '\033[5m',
        'reverse': '\033[7m',
        'hidden': '\033[8m'
    }
    
    # Caracteres para desenho de UI
    UI_CHARS = {
        'single': {
            'horizontal': '─',
            'vertical': '│',
            'top_left': '┌',
            'top_right': '┐',
            'bottom_left': '└',
            'bottom_right': '┘',
            'cross': '┼',
            't_left': '┤',
            't_right': '├',
            't_top': '┴',
            't_bottom': '┬'
        },
        'double': {
            'horizontal': '═',
            'vertical': '║',
            'top_left': '╔',
            'top_right': '╗',
            'bottom_left': '╚',
            'bottom_right': '╝',
            'cross': '╬',
            't_left': '╣',
            't_right': '╠',
            't_top': '╩',
            't_bottom': '╦'
        },
        'rounded': {
            'top_left': '╭',
            'top_right': '╮',
            'bottom_left': '╰',
            'bottom_right': '╯'
        }
    }
    
    def __init__(self):
        self.screen_width = 80
        self.screen_height = 24
        self.buffer: List[List[str]] = []
        self.clear()
        
    def clear(self):
        """Limpa o buffer de renderização"""
        self.buffer = [[' ' for _ in range(self.screen_width)] 
                      for _ in range(self.screen_height)]
    
    def set_cell(self, x: int, y: int, char: str, 
                 color: str = 'white', style: str = ''):
        """Define um caractere no buffer"""
        if 0 <= x < self.screen_width and 0 <= y < self.screen_height:
            colored_char = self.colorize(char, color, style)
            self.buffer[y][x] = colored_char
    
    def colorize(self, text: str, color: str = 'white', 
                style: str = '') -> str:
        """Aplica cores e estilos ANSI ao texto"""
        color_code = self.COLORS.get(color, self.COLORS['white'])
        style_code = self.STYLES.get(style, '')
        reset_code = self.COLORS['reset']
        
        return f"{style_code}{color_code}{text}{reset_code}"
    
    def draw_text(self, x: int, y: int, text: str, 
                  color: str = 'white', style: str = ''):
        """Desenha texto na posição especificada"""
        colored_text = self.colorize(text, color, style)
        
        for i, char in enumerate(colored_text):
            if x + i < self.screen_width and y < self.screen_height:
                self.buffer[y][x + i] = char
    
    def draw_box(self, x: int, y: int, width: int, height: int,
                style: str = 'single', color: str = 'white',
                fill: bool = False, fill_char: str = ' '):
        """Desenha uma caixa"""
        chars = self.UI_CHARS.get(style, self.UI_CHARS['single'])
        
        # Cantos
        self.set_cell(x, y, chars['top_left'], color)
        self.set_cell(x + width - 1, y, chars['top_right'], color)
        self.set_cell(x, y + height - 1, chars['bottom_left'], color)
        self.set_cell(x + width - 1, y + height - 1, chars['bottom_right'], color)
        
        # Bordas horizontais
        for i in range(1, width - 1):
            self.set_cell(x + i, y, chars['horizontal'], color)
            self.set_cell(x + i, y + height - 1, chars['horizontal'], color)
        
        # Bordas verticais
        for i in range(1, height - 1):
            self.set_cell(x, y + i, chars['vertical'], color)
            self.set_cell(x + width - 1, y + i, chars['vertical'], color)
        
        # Preenchimento
        if fill:
            for i in range(1, width - 1):
                for j in range(1, height - 1):
                    self.set_cell(x + i, y + j, fill_char, color)
    
    def draw_progress_bar(self, x: int, y: int, width: int,
                         current: float, maximum: float,
                         color: str = 'green', 
                         bg_color: str = 'white'):
        """Desenha uma barra de progresso"""
        percentage = current / maximum
        filled_width = int(width * percentage)
        
        # Fundo
        self.draw_text(x, y, '[' + ' ' * width + ']', bg_color)
        
        # Preenchimento
        if filled_width > 0:
            bar_char = '█'
            for i in range(filled_width):
                self.set_cell(x + 1 + i, y, bar_char, color)
        
        # Texto de porcentagem
        percent_text = f"{int(percentage * 100)}%"
        text_x = x + width // 2 - len(percent_text) // 2
        self.draw_text(text_x, y, percent_text, 'white', 'bold')
    
    def draw_map(self, game_map: GameMap, offset_x: int = 0, offset_y: int = 0):
        """Renderiza o mapa no buffer"""
        viewport_width = min(self.screen_width - offset_x, game_map.width)
        viewport_height = min(self.screen_height - offset_y, game_map.height)
        
        for y in range(viewport_height):
            for x in range(viewport_width):
                map_x, map_y = x, y
                tile = game_map.get_tile(map_x, map_y)
                
                if tile:
                    if tile.visible:
                        # Tile visível: mostra normalmente
                        color = tile.color
                        symbol = tile.symbol
                        
                        # Verifica se há entidade
                        entity = game_map.get_entity_at(map_x, map_y)
                        if entity:
                            symbol = entity.symbol
                            color = entity.color
                        
                        self.set_cell(offset_x + x, offset_y + y, symbol, color)
                    
                    elif tile.discovered:
                        # Tile descoberto mas não visível: mostra escurecido
                        self.set_cell(offset_x + x, offset_y + y, tile.symbol, 'bright_black')
                    
                    else:
                        # Tile não descoberto: mostra como desconhecido
                        self.set_cell(offset_x + x, offset_y + y, ' ', 'black')
    
    def draw_ui_panel(self, title: str, content: List[str],
                     x: int, y: int, width: int, height: int):
        """Desenha um painel de UI com título e conteúdo"""
        # Caixa
        self.draw_box(x, y, width, height, 'double', 'cyan', True, ' ')
        
        # Título
        title_x = x + (width - len(title)) // 2
        self.draw_text(title_x, y, title, 'yellow', 'bold')
        
        # Conteúdo
        for i, line in enumerate(content[:height - 2]):
            self.draw_text(x + 1, y + 1 + i, line, 'white')
    
    def draw_inventory(self, inventory: InventoryComponent,
                      x: int, y: int, width: int, height: int):
        """Renderiza o inventário"""
        title = "INVENTÁRIO"
        content = []
        
        # Cabeçalho
        content.append(f"Capacidade: {len(inventory.items)}/{inventory.capacity}")
        content.append(f"Peso: {inventory.get_total_weight():.1f}")
        
        if inventory.is_overencumbered():
            content.append("SOBRECARREGADO!", 'red')
        
        content.append("-" * (width - 2))
        
        # Itens
        for i, item in enumerate(inventory.items):
            if i >= height - 6:  # Limita para caber no painel
                content.append("...")
                break
            
            stack_info = ""
            if item.stackable:
                stack_info = f" x{item.current_stack}"
            
            rarity_color = {
                ItemRarity.COMMON: 'white',
                ItemRarity.UNCOMMON: 'green',
                ItemRarity.RARE: 'blue',
                ItemRarity.EPIC: 'magenta',
                ItemRarity.LEGENDARY: 'yellow'
            }.get(item.rarity, 'white')
            
            item_line = f"{item.symbol} {item.name}{stack_info}"
            content.append((item_line, rarity_color))
        
        # Equipamentos
        content.append("-" * (width - 2))
        content.append("EQUIPADO:")
        
        for slot, item in inventory.equipped.items():
            if item:
                content.append(f"  {slot}: {item.name}")
        
        # Renderiza
        self.draw_ui_panel(title, content, x, y, width, height)
    
    def draw_status(self, entity: Entity, 
                   x: int, y: int, width: int, height: int):
        """Renderiza status de uma entidade"""
        title = entity.name.upper()
        content = []
        
        # Vida
        health = entity.get_component('health')
        if health:
            hp_percent = health.get_health_percentage()
            hp_color = 'green' if hp_percent > 0.5 else \
                      'yellow' if hp_percent > 0.25 else 'red'
            
            hp_bar = '█' * int((width - 10) * hp_percent)
            content.append(f"Vida: {health.current_hp}/{health.max_hp}")
            content.append((f"[{hp_bar:<{width-10}}]", hp_color))
        
        # Experiência
        exp = entity.get_component('experience')
        if exp:
            content.append(f"Nível: {exp.level}")
            content.append(f"XP: {exp.experience}/{exp.experience_to_next}")
            content.append(f"Pontos: {exp.skill_points}")
        
        # Atributos
        if exp:
            content.append("-" * (width - 2))
            content.append("ATRIBUTOS:")
            for attr, value in exp.attributes.items():
                content.append(f"  {attr}: {value}")
        
        self.draw_ui_panel(title, content, x, y, width, height)
    
    def draw_message_log(self, messages: List[str],
                        x: int, y: int, width: int, height: int,
                        title: str = "MENSAGENS"):
        """Renderiza log de mensagens"""
        # Mostra apenas as últimas mensagens que cabem
        visible_messages = messages[-(height - 2):]
        
        self.draw_ui_panel(title, visible_messages, x, y, width, height)
    
    def render(self):
        """Renderiza o buffer na tela"""
        os.system('cls' if os.name == 'nt' else 'clear')
        
        for row in self.buffer:
            print(''.join(row))
    
    def animate_text(self, text: str, x: int, y: int, 
                    delay: float = 0.05, color: str = 'white'):
        """Animação de texto aparecendo caractere por caractere"""
        for i, char in enumerate(text):
            self.set_cell(x + i, y, char, color)
            self.render()
            time.sleep(delay)
    
    def fade_in(self, steps: int = 10, char: str = '█', 
               color: str = 'black'):
        """Efeito de fade in"""
        temp_buffer = [row.copy() for row in self.buffer]
        
        for step in range(steps):
            alpha = step / steps
            
            for y in range(self.screen_height):
                for x in range(self.screen_width):
                    if self.buffer[y][x] == ' ':
                        self.set_cell(x, y, char, color)
            
            self.render()
            time.sleep(0.1)
        
        # Restaura buffer original
        self.buffer = temp_buffer
    
    def shake_screen(self, intensity: int = 1, duration: float = 0.5):
        """Efeito de tremor na tela"""
        import random
        
        original_buffer = [row.copy() for row in self.buffer]
        start_time = time.time()
        
        while time.time() - start_time < duration:
            dx = random.randint(-intensity, intensity)
            dy = random.randint(-intensity, intensity)
            
            temp_buffer = [[' ' for _ in range(self.screen_width)]
                          for _ in range(self.screen_height)]
            
            for y in range(self.screen_height):
                for x in range(self.screen_width):
                    nx, ny = x + dx, y + dy
                    if 0 <= nx < self.screen_width and 0 <= ny < self.screen_height:
                        temp_buffer[ny][nx] = original_buffer[y][x]
            
            self.buffer = temp_buffer
            self.render()
            time.sleep(0.05)
        
        # Restaura buffer original
        self.buffer = original_buffer
```

### 2. Sistema de Input
```python
import sys
import tty
import termios
import select

class TerminalInput:
    """Sistema de entrada para terminal"""
    
    # Mapeamento de teclas
    KEYS = {
        'UP': ['w', 'W', 'k', 'K', '\x1b[A'],  # ↑
        'DOWN': ['s', 'S', 'j', 'J', '\x1b[B'],  # ↓
        'LEFT': ['a', 'A', 'h', 'H', '\x1b[D'],  # ←
        'RIGHT': ['d', 'D', 'l', 'L', '\x1b[C'],  # →
        'CONFIRM': [' ', '\n', '\r'],
        'CANCEL': ['q', 'Q', '\x1b', '\x03'],  # ESC ou Ctrl+C
        'INVENTORY': ['i', 'I'],
        'CHARACTER': ['c', 'C'],
        'USE': ['u', 'U'],
        'PICKUP': ['p', 'P'],
        'EXAMINE': ['x', 'X'],
        'WAIT': ['.', '5'],
        'SAVE': ['F5'],
        'LOAD': ['F9']
    }
    
    def __init__(self):
        self.old_settings = None
    
    def enable_raw_mode(self):
        """Ativa modo raw para captura de teclas"""
        self.old_settings = termios.tcgetattr(sys.stdin)
        tty.setraw(sys.stdin.fileno())
    
    def disable_raw_mode(self):
        """Desativa modo raw"""
        if self.old_settings:
            termios.tcsetattr(sys.stdin, termios.TCSADRAIN, self.old_settings)
    
    def get_key(self, timeout: float = 0.1) -> Optional[str]:
        """Obtém uma tecla pressionada"""
        if select.select([sys.stdin], [], [], timeout)[0]:
            return sys.stdin.read(1)
        return None
    
    def get_input(self) -> str:
        """Obtém entrada do usuário"""
        self.enable_raw_mode()
        
        try:
            key = self.get_key()
            if key == '\x1b':  # Escape sequence
                # Tenta ler mais caracteres para setas, F-keys, etc.
                more_key = self.get_key(0.01)
                if more_key:
                    key += more_key
                    more_key2 = self.get_key(0.01)
                    if more_key2:
                        key += more_key2
            
            return key if key else ''
        
        finally:
            self.disable_raw_mode()
    
    def translate_key(self, key: str) -> Optional[str]:
        """Traduz código de tecla para ação"""
        for action, key_list in self.KEYS.items():
            if key in key_list:
                return action
        
        # Teclas normais
        if key and key.isprintable():
            return key.upper()
        
        return None
    
    def wait_for_key(self, valid_keys: List[str] = None) -> str:
        """Aguarda por uma tecla específica"""
        while True:
            key = self.get_input()
            action = self.translate_key(key)
            
            if valid_keys is None or action in valid_keys:
                return action
    
    def get_direction_from_key(self, key_action: str) -> Optional[Vector2D]:
        """Converte ação de tecla para direção"""
        directions = {
            'UP': Vector2D(0, -1),
            'DOWN': Vector2D(0, 1),
            'LEFT': Vector2D(-1, 0),
            'RIGHT': Vector2D(1, 0),
            'UP_LEFT': Vector2D(-1, -1),
            'UP_RIGHT': Vector2D(1, -1),
            'DOWN_LEFT': Vector2D(-1, 1),
            'DOWN_RIGHT': Vector2D(1, 1)
        }
        
        return directions.get(key_action)
    
    def get_number_input(self, min_val: int = 0, max_val: int = 9) -> Optional[int]:
        """Obtém entrada numérica"""
        print(f"Digite um número ({min_val}-{max_val}): ", end='', flush=True)
        
        while True:
            key = self.get_input()
            
            if key in ['\x1b', '\x03']:  # ESC ou Ctrl+C
                print()
                return None
            
            if key.isdigit():
                num = int(key)
                if min_val <= num <= max_val:
                    print(key)
                    return num
            
            print("\b \b", end='', flush=True)  # Apaga caractere inválido
    
    def get_text_input(self, prompt: str = "", max_length: int = 20) -> str:
        """Obtém entrada de texto"""
        print(prompt, end='', flush=True)
        
        text = ""
        cursor_pos = 0
        
        while True:
            key = self.get_input()
            
            if key in ['\n', '\r']:  # Enter
                print()
                return text
            
            elif key in ['\x1b', '\x03']:  # ESC ou Ctrl+C
                print()
                return ""
            
            elif key == '\x7f' or key == '\x08':  # Backspace
                if cursor_pos > 0:
                    text = text[:cursor_pos-1] + text[cursor_pos:]
                    cursor_pos -= 1
                    print(f"\r{prompt}{text} \b", end='', flush=True)
            
            elif key == '\x1b[D':  # ←
                if cursor_pos > 0:
                    cursor_pos -= 1
                    print(f"\r{prompt}{text}", end='', flush=True)
                    print(f"\b" * (len(text) - cursor_pos), end='', flush=True)
            
            elif key == '\x1b[C':  # →
                if cursor_pos < len(text):
                    cursor_pos += 1
                    print(f"\r{prompt}{text}", end='', flush=True)
                    print(f"\b" * (len(text) - cursor_pos), end='', flush=True)
            
            elif key.isprintable() and len(text) < max_length:
                text = text[:cursor_pos] + key + text[cursor_pos:]
                cursor_pos += 1
                print(f"\r{prompt}{text}", end='', flush=True)
                print(f"\b" * (len(text) - cursor_pos), end='', flush=True)
```

---

## 🎮 EXEMPLOS COMPLETOS

### 1. Jogo de Roguelike Básico
```python
#!/usr/bin/env python3
"""
Exemplo completo de um roguelike no terminal.
"""

import sys
import time
from typing import Optional

class RogueLikeGame:
    """Jogo roguelike básico"""
    
    def __init__(self):
        self.renderer = TerminalRenderer()
        self.input_handler = TerminalInput()
        self.game_state = GameState()
        self.running = True
        self.messages = []
        self.game_mode = "explore"
        
        # Configurações da tela
        self.renderer.screen_width = 100
        self.renderer.screen_height = 40
        
        # Áreas da interface
        self.map_area = {
            'x': 0, 'y': 0,
            'width': 80, 'height': 30
        }
        self.status_area = {
            'x': 80, 'y': 0,
            'width': 20, 'height': 15
        }
        self.message_area = {
            'x': 0, 'y': 30,
            'width': 100, 'height': 10
        }
    
    def add_message(self, message: str, color: str = 'white'):
        """Adiciona uma mensagem ao log"""
        self.messages.append((message, color))
        if len(self.messages) > 100:  # Limita histórico
            self.messages.pop(0)
    
    def initialize(self):
        """Inicializa o jogo"""
        print("Inicializando jogo...")
        
        # Obtém nome do jogador
        self.renderer.clear()
        self.renderer.draw_text(10, 10, "QUAL É O SEU NOME, AVENTUREIRO?", 'yellow', 'bold')
        self.renderer.render()
        
        player_name = self.input_handler.get_text_input("> ", 20)
        if not player_name:
            player_name = "Herói"
        
        # Inicializa estado do jogo
        self.game_state.initialize_new_game(player_name)
        
        # Mensagem de boas-vindas
        self.add_message(f"Bem-vindo(a), {player_name}!", 'yellow')
        self.add_message("Use WASD ou setas para mover.", 'cyan')
        self.add_message("Pressione 'i' para inventário.", 'cyan')
        self.add_message("Pressione 'q' para sair.", 'cyan')
        
        # Primeira renderização
        self.render()
    
    def render(self):
        """Renderiza a tela completa"""
        self.renderer.clear()
        
        # Renderiza mapa
        if self.game_state.current_map:
            self.renderer.draw_map(
                self.game_state.current_map,
                self.map_area['x'],
                self.map_area['y']
            )
        
        # Renderiza status do jogador
        if self.game_state.player:
            self.renderer.draw_status(
                self.game_state.player,
                self.status_area['x'],
                self.status_area['y'],
                self.status_area['width'],
                self.status_area['height']
            )
        
        # Renderiza inventário se aberto
        if self.game_mode == "inventory":
            inventory = self.game_state.player.get_component('inventory')
            if inventory:
                self.renderer.draw_inventory(
                    inventory,
                    self.map_area['x'] + 10,
                    self.map_area['y'] + 5,
                    60, 20
                )
        
        # Renderiza log de mensagens
        message_texts = [msg for msg, _ in self.messages[-8:]]
        self.renderer.draw_message_log(
            message_texts,
            self.message_area['x'],
            self.message_area['y'],
            self.message_area['width'],
            self.message_area['height']
        )
        
        # Renderiza na tela
        self.renderer.render()
    
    def handle_input(self):
        """Processa entrada do usuário"""
        key = self.input_handler.get_input()
        action = self.input_handler.translate_key(key)
        
        if not action:
            return
        
        # Teclas globais
        if action == 'CANCEL':
            if self.game_mode == "inventory":
                self.game_mode = "explore"
                return
            self.running = False
            return
        
        # Modo exploração
        if self.game_mode == "explore":
            self.handle_exploration_input(action)
        
        # Modo inventário
        elif self.game_mode == "inventory":
            self.handle_inventory_input(action)
    
    def handle_exploration_input(self, action: str):
        """Processa entrada no modo exploração"""
        player = self.game_state.player
        game_map = self.game_state.current_map
        
        if not player or not game_map:
            return
        
        # Movimento
        direction = self.input_handler.get_direction_from_key(action)
        if direction:
            self.move_player(direction)
            self.game_state.update()
            return
        
        # Ações
        if action == 'INVENTORY':
            self.game_mode = "inventory"
            self.add_message("Inventário aberto.", 'cyan')
        
        elif action == 'PICKUP':
            self.pickup_item()
            self.game_state.update()
        
        elif action == 'USE':
            self.use_item()
            self.game_state.update()
        
        elif action == 'WAIT':
            self.add_message("Você espera...", 'white')
            self.game_state.update()
        
        elif action == 'EXAMINE':
            self.examine()
        
        elif action == 'SAVE':
            self.save_game()
        
        elif action == 'LOAD':
            self.load_game()
    
    def handle_inventory_input(self, action: str):
        """Processa entrada no modo inventário"""
        # Em implementação completa, aqui teria navegação no inventário
        pass
    
    def move_player(self, direction: Vector2D):
        """Move o jogador na direção especificada"""
        player = self.game_state.player
        game_map = self.game_state.current_map
        
        if not player or not game_map:
            return
        
        new_pos = player.position + direction
        
        # Verifica se pode mover
        if not game_map.is_walkable(new_pos.x, new_pos.y):
            # Verifica se há uma porta
            tile = game_map.get_tile(new_pos.x, new_pos.y)
            if tile and tile.tile_type == TileType.DOOR_CLOSED:
                tile.tile_type = TileType.DOOR_OPEN
                tile.walkable = True
                tile.transparent = True
                self.add_message("Você abriu a porta.", 'yellow')
                return
            else:
                self.add_message("Você não pode passar por aí.", 'white')
                return
        
        # Verifica se há entidade no tile
        entity = game_map.get_entity_at(new_pos.x, new_pos.y)
        if entity:
            if entity.has_tag('enemy'):
                # Inicia combate
                self.add_message(f"Você ataca {entity.name}!", 'red')
                self.attack_entity(entity)
                return
            elif entity.has_tag('item'):
                # Pega item
                self.pickup_item_at(new_pos)
                return
        
        # Move jogador
        game_map.move_entity(player.id, new_pos)
        
        # Atualiza campo de visão
        update_fov(game_map, player)
        
        # Evento de movimento
        self.game_state.event_system.emit(GameEvent(
            "player_move",
            source=player,
            data={'position': new_pos}
        ))
    
    def attack_entity(self, target: Entity):
        """Ataca uma entidade"""
        player = self.game_state.player
        
        if not player:
            return
        
        combat = player.get_component('combat')
        if not combat:
            return
        
        result = combat.attack(target)
        
        if result.get('success'):
            damage = result.get('damage', 0)
            is_critical = result.get('is_critical', False)
            target_alive = result.get('target_alive', True)
            
            crit_text = " CRÍTICO!" if is_critical else ""
            
            if target_alive:
                self.add_message(
                    f"Você causa {damage} de dano em {target.name}!{crit_text}",
                    'red'
                )
            else:
                self.add_message(
                    f"Você derrota {target.name} causando {damage} de dano!{crit_text}",
                    'green'
                )
                
                # XP
                exp = player.get_component('experience')
                if exp:
                    xp_gained = exp.get_xp_from_enemy(target)
                    exp.add_experience(xp_gained)
                    self.add_message(f"Você ganha {xp_gained} pontos de experiência!", 'yellow')
                
                # Remove entidade morta
                self.game_state.current_map.remove_entity(target.id)
        else:
            self.add_message(f"Você erra o ataque em {target.name}!", 'white')
    
    def pickup_item(self):
        """Pega item na posição atual do jogador"""
        player = self.game_state.player
        game_map = self.game_state.current_map
        
        if not player or not game_map:
            return
        
        # Encontra itens na posição do jogador
        tile = game_map.get_tile(player.position.x, player.position.y)
        if not tile:
            return
        
        items = tile.get_items()
        if not items:
            # Verifica se há entidade item
            entities = [e for e in game_map.entities.values() 
                       if e.position == player.position and e.type == EntityType.ITEM]
            
            if not entities:
                self.add_message("Não há nada para pegar aqui.", 'white')
                return
            
            for entity in entities:
                self.pickup_entity_item(entity)
            
            return
        
        # Pega itens do tile
        for item in items:
            inventory = player.get_component('inventory')
            if inventory and inventory.add_item(item):
                tile.remove_item(item)
                self.add_message(f"Você pega {item.name}.", 'green')
            else:
                self.add_message(f"Inventário cheio! Não pode pegar {item.name}.", 'red')
    
    def pickup_entity_item(self, entity: Entity):
        """Pega um item que é uma entidade"""
        player = self.game_state.player
        
        if not player:
            return
        
        item_component = entity.get_component('item')
        if not item_component:
            return
        
        # Cria item real a partir do componente
        item = ItemFactory.create_from_entity(entity)
        
        inventory = player.get_component('inventory')
        if inventory and inventory.add_item(item):
            self.game_state.current_map.remove_entity(entity.id)
            self.add_message(f"Você pega {item.name}.", 'green')
        else:
            self.add_message(f"Inventário cheio! Não pode pegar {item.name}.", 'red')
    
    def use_item(self):
        """Usa um item do inventário"""
        player = self.game_state.player
        
        if not player:
            return
        
        # Em implementação completa, aqui teria seleção de item
        self.add_message("Selecione um item para usar (implementação pendente).", 'cyan')
    
    def examine(self):
        """Examina a posição atual"""
        player = self.game_state.player
        game_map = self.game_state.current_map
        
        if not player or not game_map:
            return
        
        pos = player.position
        tile = game_map.get_tile(pos.x, pos.y)
        
        if not tile:
            return
        
        # Descreve o tile
        tile_names = {
            TileType.FLOOR: "chão de pedra",
            TileType.WALL: "parede de pedra",
            TileType.WATER: "água",
            TileType.LAVA: "lava",
            TileType.GRASS: "grama",
            TileType.DOOR_OPEN: "porta aberta",
            TileType.DOOR_CLOSED: "porta fechada",
            TileType.STAIRS_UP: "escada para cima",
            TileType.STAIRS_DOWN: "escada para baixo"
        }
        
        tile_desc = tile_names.get(tile.tile_type, "desconhecido")
        
        # Verifica se há itens
        items = tile.get_items()
        items_desc = ""
        if items:
            item_names = ", ".join(item.name for item in items)
            items_desc = f" Você vê: {item_names}."
        
        # Verifica se há entidades
        entities = [e for e in game_map.entities.values() 
                   if e.position == pos and e != player]
        entities_desc = ""
        if entities:
            entity_names = ", ".join(e.name for e in entities)
            entities_desc = f" Há aqui: {entity_names}."
        
        self.add_message(
            f"Você está em um {tile_desc}.{items_desc}{entities_desc}",
            'cyan'
        )
    
    def save_game(self):
        """Salva o jogo"""
        save_data = self.game_state.save_game()
        
        save_system = SaveSystem()
        save_id = save_system.create_save(save_data, "Save manual")
        
        self.add_message(f"Jogo salvo com ID: {save_id}", 'green')
    
    def load_game(self):
        """Carrega um jogo salvo"""
        save_system = SaveSystem()
        saves = save_system.list_saves()
        
        if not saves:
            self.add_message("Não há saves disponíveis.", 'red')
            return
        
        # Em implementação completa, aqui teria menu de seleção
        self.add_message("Selecione um save para carregar (implementação pendente).", 'cyan')
    
    def run(self):
        """Loop principal do jogo"""
        self.initialize()
        
        try:
            while self.running:
                self.render()
                self.handle_input()
        
        except KeyboardInterrupt:
            print("\nJogo interrompido.")
        
        finally:
            self.input_handler.disable_raw_mode()
            print("Obrigado por jogar!")

if __name__ == "__main__":
    game = RogueLikeGame()
    game.run()
```

### 2. Sistema de Combate por Turnos
```python
class TurnBasedCombat:
    """Sistema de combate por turnos"""
    
    def __init__(self):
        self.combatants = []
        self.turn_order = []
        self.current_turn = 0
        self.in_combat = False
    
    def start_combat(self, participants: List[Entity]):
        """Inicia um combate"""
        self.combatants = participants
        self.in_combat = True
        
        # Calcula iniciativa
        initiatives = []
        for entity in participants:
            initiative = self.calculate_initiative(entity)
            initiatives.append((initiative, entity))
        
        # Ordena por iniciativa (maior primeiro)
        initiatives.sort(reverse=True, key=lambda x: x[0])
        self.turn_order = [entity for _, entity in initiatives]
        
        self.current_turn = 0
        self.print_combat_start()
    
    def calculate_initiative(self, entity: Entity) -> int:
        """Calcula iniciativa de uma entidade"""
        base = 10
        
        # Modificadores
        combat = entity.get_component('combat')
        if combat:
            base += combat.attack_power // 5
            base -= combat.defense // 3
        
        # Aleatoriedade
        import random
        base += random.randint(1, 20)
        
        return base
    
    def print_combat_start(self):
        """Mostra início do combate"""
        print("\n" + "=" * 50)
        print("COMBATE INICIADO!")
        print("Ordem de turnos:")
        
        for i, entity in enumerate(self.turn_order, 1):
            print(f"  {i}. {entity.name}")
        
        print("=" * 50)
    
    def next_turn(self):
        """Avança para o próximo turno"""
        self.current_turn = (self.current_turn + 1) % len(self.turn_order)
        
        # Remove combatantes mortos
        self.remove_dead_combatants()
        
        # Verifica se o combate acabou
        if len(self.combatants) <= 1:
            self.end_combat()
            return
        
        # Mostra turno atual
        current = self.turn_order[self.current_turn]
        print(f"\nTurno de: {current.name}")
    
    def remove_dead_combatants(self):
        """Remove combatantes mortos da ordem de turnos"""
        alive_combatants = []
        alive_turn_order = []
        
        for entity in self.combatants:
            health = entity.get_component('health')
            if health and health.is_alive:
                alive_combatants.append(entity)
        
        for entity in self.turn_order:
            if entity in alive_combatants:
                alive_turn_order.append(entity)
        
        self.combatants = alive_combatants
        self.turn_order = alive_turn_order
        
        # Ajusta índice do turno atual
        if self.current_turn >= len(self.turn_order):
            self.current_turn = 0
    
    def get_current_combatant(self) -> Optional[Entity]:
        """Retorna a entidade do turno atual"""
        if not self.turn_order:
            return None
        return self.turn_order[self.current_turn]
    
    def execute_action(self, action: str, **kwargs) -> Dict[str, Any]:
        """Executa uma ação no combate"""
        current = self.get_current_combatant()
        if not current:
            return {'success': False, 'message': 'Nenhum combatante ativo'}
        
        if action == 'attack':
            target = kwargs.get('target')
            if not target:
                return {'success': False, 'message': 'Nenhum alvo'}
            
            combat = current.get_component('combat')
            if not combat:
                return {'success': False, 'message': 'Não pode atacar'}
            
            result = combat.attack(target)
            self.next_turn()
            return result
        
        elif action == 'use_item':
            item_id = kwargs.get('item_id')
            target = kwargs.get('target', current)
            
            inventory = current.get_component('inventory')
            if not inventory:
                return {'success': False, 'message': 'Sem inventário'}
            
            item = inventory.get_item(item_id)
            if not item:
                return {'success': False, 'message': 'Item não encontrado'}
            
            result = item.use(current, target)
            
            if result.get('success') and item.category == ItemCategory.CONSUMABLE:
                inventory.remove_item(item_id)
            
            self.next_turn()
            return result
        
        elif action == 'flee':
            import random
            success = random.random() > 0.3  # 70% de chance
            
            if success:
                self.end_combat()
                return {'success': True, 'message': 'Fuga bem sucedida'}
            else:
                self.next_turn()
                return {'success': False, 'message': 'Falha na fuga'}
        
        self.next_turn()
        return {'success': False, 'message': 'Ação desconhecida'}
    
    def end_combat(self):
        """Finaliza o combate"""
        self.in_combat = False
        self.combatants.clear()
        self.turn_order.clear()
        
        print("\n" + "=" * 50)
        print("COMBATE ENCERRADO!")
        print("=" * 50)
    
    def is_player_turn(self) -> bool:
        """Verifica se é o turno do jogador"""
        current = self.get_current_combatant()
        return current and current.has_tag('player')
    
    def update(self):
        """Atualiza o sistema de combate"""
        if not self.in_combat:
            return
        
        # Se não é turno do jogador, executa IA
        if not self.is_player_turn():
            current = self.get_current_combatant()
            if current and current.has_tag('enemy'):
                self.execute_enemy_turn(current)
    
    def execute_enemy_turn(self, enemy: Entity):
        """Executa o turno de um inimigo controlado por IA"""
        ai = enemy.get_component('ai')
        if not ai:
            # Se não tem IA, apenas ataca um alvo aleatório
            self.attack_random_target(enemy)
            return
        
        # Encontra alvo (normalmente o jogador)
        player = next((e for e in self.combatants if e.has_tag('player')), None)
        if not player:
            self.attack_random_target(enemy)
            return
        
        # Executa ação baseada na IA
        if ai.ai_type == "aggressive":
            self.execute_action('attack', target=player)
        elif ai.ai_type == "coward":
            health = enemy.get_component('health')
            if health and health.get_health_percentage() < 0.3:
                self.execute_action('flee')
            else:
                self.execute_action('attack', target=player)
        else:
            self.execute_action('attack', target=player)
    
    def attack_random_target(self, attacker: Entity):
        """Ataca um alvo aleatório"""
        # Encontra alvos válidos (não o próprio atacante)
        targets = [e for e in self.combatants 
                  if e != attacker and e.get_component('health')]
        
        if targets:
            import random
            target = random.choice(targets)
            self.execute_action('attack', target=target)
        else:
            self.next_turn()
```

---

## 🏗️ PADRÕES DE DESIGN

### 1. Component Pattern (ECS - Entity Component System)
```python
"""
Padrão Component: Separa comportamento de dados.
Entidades são apenas IDs, componentes são dados, sistemas são comportamento.
"""

class ECS:
    """Sistema de Entidade-Componente-Sistema simplificado"""
    
    def __init__(self):
        self.next_entity_id = 0
        self.entities = {}
        self.components = {}
        self.systems = []
    
    def create_entity(self) -> int:
        """Cria uma nova entidade"""
        entity_id = self.next_entity_id
        self.next_entity_id += 1
        self.entities[entity_id] = {}
        return entity_id
    
    def add_component(self, entity_id: int, component_type: str, component_data: Any):
        """Adiciona componente a uma entidade"""
        if entity_id not in self.entities:
            return
        
        self.entities[entity_id][component_type] = component_data
        
        if component_type not in self.components:
            self.components[component_type] = {}
        self.components[component_type][entity_id] = component_data
    
    def get_component(self, entity_id: int, component_type: str) -> Optional[Any]:
        """Obtém componente de uma entidade"""
        return self.entities.get(entity_id, {}).get(component_type)
    
    def get_entities_with(self, *component_types: str) -> List[int]:
        """Obtém entidades que possuem todos os componentes especificados"""
        result = []
        
        for entity_id, components in self.entities.items():
            if all(comp_type in components for comp_type in component_types):
                result.append(entity_id)
        
        return result
    
    def add_system(self, system):
        """Adiciona um sistema"""
        self.systems.append(system)
    
    def update(self):
        """Atualiza todos os sistemas"""
        for system in self.systems:
            system.update(self)

# Exemplo de uso do ECS
class MovementSystem:
    """Sistema de movimento"""
    
    def update(self, ecs: ECS):
        """Atualiza posição de entidades com componentes Position e Velocity"""
        entities = ecs.get_entities_with('position', 'velocity')
        
        for entity_id in entities:
            position = ecs.get_component(entity_id, 'position')
            velocity = ecs.get_component(entity_id, 'velocity')
            
            # Atualiza posição
            position.x += velocity.x
            position.y += velocity.y
            
            # Atualiza componente
            ecs.add_component(entity_id, 'position', position)
```

### 2. Observer Pattern (Sistema de Eventos)
```python
"""
Padrão Observer: Implementado no EventSystem acima.
Permite que objetos se inscrevam em eventos e sejam notificados quando ocorrem.
"""

class Observer:
    """Interface para observadores"""
    
    def on_notify(self, event):
        pass

class Observable:
    """Classe observável"""
    
    def __init__(self):
        self._observers = []
    
    def add_observer(self, observer: Observer):
        self._observers.append(observer)
    
    def remove_observer(self, observer: Observer):
        if observer in self._observers:
            self._observers.remove(observer)
    
    def notify_observers(self, event):
        for observer in self._observers:
            observer.on_notify(event)

# Exemplo de uso
class HealthBar(Observer):
    """Barra de vida que observa mudanças na vida"""
    
    def __init__(self, entity: Entity):
        self.entity = entity
        entity.add_observer(self)
    
    def on_notify(self, event):
        if event.event_type == "health_changed":
            self.update_display()

class HealthComponent(Component, Observable):
    """Componente de vida que é observável"""
    
    def take_damage(self, amount: int):
        self.current_hp -= amount
        self.notify_observers(GameEvent("health_changed", damage=amount))
```

### 3. Factory Pattern (Fábrica de Entidades)
```python
"""
Padrão Factory: Centraliza criação de objetos complexos.
Implementado na EntityFactory acima.
"""

class EntityFactory:
    """Fábrica de entidades"""
    
    _templates = {
        'goblin': {
            'name': 'Goblin',
            'symbol': 'g',
            'color': 'green',
            'components': {
                'health': {'max_hp': 30},
                'combat': {'attack_power': 8, 'defense': 3},
                'ai': {'type': 'basic'}
            }
        },
        'player': {
            'name': 'Player',
            'symbol': '@',
            'color': 'yellow',
            'components': {
                'health': {'max_hp': 100},
                'combat': {'attack_power': 15, 'defense': 8},
                'inventory': {'capacity': 20}
            }
        }
    }
    
    @classmethod
    def create(cls, template_name: str, position: Vector2D) -> Entity:
        """Cria entidade a partir de template"""
        if template_name not in cls._templates:
            raise ValueError(f"Template desconhecido: {template_name}")
        
        template = cls._templates[template_name]
        
        # Cria entidade básica
        entity = Entity(
            entity_id=f"{template_name}_{uuid.uuid4().hex[:8]}",
            name=template['name'],
            entity_type=EntityType.ENEMY if template_name != 'player' else EntityType.PLAYER,
            position=position,
            symbol=template['symbol'],
            color=template['color']
        )
        
        # Adiciona componentes
        for comp_name, comp_config in template.get('components', {}).items():
            component = cls._create_component(comp_name, entity, comp_config)
            entity.add_component(comp_name, component)
        
        return entity
    
    @classmethod
    def _create_component(cls, comp_name: str, owner: Entity, config: Dict[str, Any]) -> Component:
        """Cria componente específico"""
        if comp_name == 'health':
            return HealthComponent(owner, **config)
        elif comp_name == 'combat':
            return CombatComponent(owner, **config)
        elif comp_name == 'ai':
            return AIComponent(owner, **config)
        elif comp_name == 'inventory':
            return InventoryComponent(owner, **config)
        
        raise ValueError(f"Componente desconhecido: {comp_name}")
```

### 4. State Pattern (Máquina de Estados)
```python
"""
Padrão State: Gerencia estados diferentes de uma entidade.
"""

from abc import ABC, abstractmethod

class EntityState(ABC):
    """Classe base para estados de entidade"""
    
    def __init__(self, entity: Entity):
        self.entity = entity
    
    @abstractmethod
    def enter(self):
        """Chamado ao entrar no estado"""
        pass
    
    @abstractmethod
    def exit(self):
        """Chamado ao sair do estado"""
        pass
    
    @abstractmethod
    def update(self, game_map: GameMap):
        """Atualiza estado"""
        pass

class IdleState(EntityState):
    """Estado de inatividade"""
    
    def enter(self):
        self.entity.symbol = 'i'
    
    def exit(self):
        pass
    
    def update(self, game_map: GameMap):
        # Verifica se vê o jogador
        player = game_map.get_entity_by_type(EntityType.PLAYER)
        if player:
            distance = self.entity.position.distance_to(player.position)
            if distance < 5:
                # Muda para estado de perseguição
                self.entity.state_machine.change_state(ChasingState(self.entity))

class ChasingState(EntityState):
    """Estado de perseguição"""
    
    def enter(self):
        self.entity.symbol = 'c'
        self.entity.color = 'red'
    
    def exit(self):
        self.entity.color = 'green'
    
    def update(self, game_map: GameMap):
        player = game_map.get_entity_by_type(EntityType.PLAYER)
        if not player:
            self.entity.state_machine.change_state(IdleState(self.entity))
            return
        
        # Move-se em direção ao jogador
        direction = Vector2D(
            1 if player.position.x > self.entity.position.x else -1,
            1 if player.position.y > self.entity.position.y else -1
        )
        
        new_pos = self.entity.position + direction
        if game_map.is_walkable(new_pos.x, new_pos.y):
            game_map.move_entity(self.entity.id, new_pos)
        
        # Se está adjacente, ataca
        if self.entity.position.distance_to(player.position) <= 1.5:
            self.entity.state_machine.change_state(AttackingState(self.entity))

class AttackingState(EntityState):
    """Estado de ataque"""
    
    def enter(self):
        self.entity.symbol = 'a'
    
    def exit(self):
        pass
    
    def update(self, game_map: GameMap):
        player = game_map.get_entity_by_type(EntityType.PLAYER)
        if not player:
            self.entity.state_machine.change_state(IdleState(self.entity))
            return
        
        # Ataca o jogador
        combat = self.entity.get_component('combat')
        if combat:
            combat.attack(player)
        
        # Volta para perseguição
        self.entity.state_machine.change_state(ChasingState(self.entity))

class StateMachine:
    """Máquina de estados"""
    
    def __init__(self, initial_state: EntityState):
        self.current_state = initial_state
        self.current_state.enter()
    
    def change_state(self, new_state: EntityState):
        """Muda para um novo estado"""
        self.current_state.exit()
        self.current_state = new_state
        self.current_state.enter()
    
    def update(self, game_map: GameMap):
        """Atualiza estado atual"""
        self.current_state.update(game_map)

# Uso na entidade
class SmartEntity(Entity):
    """Entidade com máquina de estados"""
    
    def __init__(self, **kwargs):
        super().__init__(**kwargs)
        self.state_machine = StateMachine(IdleState(self))
    
    def update(self, game_map: GameMap):
        self.state_machine.update(game_map)
```

---

## 🎯 CONCLUSÃO

Este manual apresenta estruturas de dados e padrões de design essenciais para criar jogos complexos no terminal usando Python. As estruturas apresentadas são:

### Principais Componentes:

1. **Estruturas Fundamentais**: Vector2D, sistemas de coordenadas
2. **Mapas e Grids**: Representação de mundos, geração procedural
3. **Entidades e Componentes**: Sistema ECS para comportamento modular
4. **Inventário e Itens**: Sistema completo de itens com raridade, categorias
5. **Combate**: Sistemas de turnos, cálculo de dano, IA
6. **Progressão**: Experiência, níveis, missões
7. **Persistência**: Salvamento e carregamento de jogos
8. **Pathfinding e IA**: A*, campos de visão, comportamento inteligente
9. **Interface**: Renderização no terminal, entrada de usuário
10. **Design Patterns**: Padrões reutilizáveis para código limpo

### Boas Práticas:

1. **Separação de Responsabilidades**: Cada classe/componente tem uma única responsabilidade
2. **Composição sobre Herança**: Use componentes em vez de hierarquias profundas
3. **Serialização Completa**: Todas as estruturas podem ser salvas e carregadas
4. **Sistema de Eventos**: Comunicação desacoplada entre componentes
5. **Configuração por Dados**: Balanceamento fácil via arquivos de configuração

### Próximos Passos:

1. **Otimização**: Implementar spatial hashing para entidades
2. **Networking**: Adicionar multiplayer
3. **Modding**: Sistema para mods e conteúdo gerado por usuários
4. **UI Avançada**: Menus, diálogos, janelas modais
5. **Efeitos**: Partículas, animações, sons

Com essas estruturas, você pode criar desde jogos simples de texto até roguelikes complexos com múltiplos sistemas interconectados. A modularidade permite começar pequeno e expandir gradualmente.

**Dica**: Comece com o básico (mapa + jogador + movimento) e vá adicionando sistemas conforme necessário. Cada componente é independente e pode ser desenvolvido separadamente.

Boa codificação e divirta-se criando seus jogos! 🎮
