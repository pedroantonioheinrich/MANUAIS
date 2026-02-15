# Manual Completo de Django: Guia Prático com Explicações Detalhadas

## 📚 Índice
1. [Introdução ao Django](#introdução)
2. [Configuração do Ambiente](#configuracao)
3. [Estrutura de um Projeto Django](#estrutura)
4. [Models: O Coração da Aplicação](#models)
5. [Views: A Lógica de Negócio](#views)
6. [Templates: A Camada de Apresentação](#templates)
7. [URLs: O Roteamento](#urls)
8. [Forms: Manipulação de Dados](#forms)
9. [Admin: Interface Administrativa](#admin)
10. [Autenticação e Autorização](#auth)
11. [APIs com Django REST Framework](#api)
12. [Exemplo Prático Completo](#exemplo)

---

## 1. Introdução ao Django <a name="introducao"></a>

### O que é Django?

Django é um framework web de alto nível em Python que incentiva o desenvolvimento rápido e design limpo e pragmático. Criado em 2003 por Adrian Holovaty e Simon Willison para o jornal Lawrence Journal-World, foi liberado publicamente em 2005.

**Por que Django existe?**
Para resolver o problema comum em desenvolvimento web: repetição de tarefas. Desenvolvedores web enfrentam os mesmos desafios repetidamente - autenticação, admin, ORM, etc. Django fornece soluções prontas para que você foque no que torna sua aplicação única.

### Filosofia "Batteries Included"

Django vem com tudo que você precisa para construir aplicações web:
- **ORM (Object-Relational Mapping)** - Trabalhe com banco de dados usando Python
- **Sistema de templates** - Separe lógica de apresentação
- **Formulários** - Geração e validação
- **Admin automático** - Interface CRUD pronta
- **Autenticação** - Sistema de usuários completo
- **Internacionalização** - Suporte a múltiplos idiomas

## 2. Configuração do Ambiente <a name="configuracao"></a>

### Por que usar ambiente virtual?

Ambientes virtuais isolam dependências do seu projeto. Sem eles, diferentes projetos poderiam exigir versões conflitantes de bibliotecas.

```bash
# Criar ambiente virtual
python -m venv meu_ambiente

# Ativar (Windows)
meu_ambiente\Scripts\activate

# Ativar (Linux/Mac)
source meu_ambiente/bin/activate

# Instalar Django
pip install django

# Verificar instalação
python -m django --version
```

**Analogia:** Pense em ambiente virtual como um contêiner separado para cada projeto - como ter cozinhas separadas para preparar pratos diferentes, evitando que ingredientes (bibliotecas) se misturem.

## 3. Estrutura de um Projeto Django <a name="estrutura"></a>

### Criando o projeto

```bash
django-admin startproject meuprojeto
cd meuprojeto
python manage.py startapp meuapp
```

### Entendendo os arquivos

```
meuprojeto/
├── manage.py          # Utilitário de linha de comando
├── meuprojeto/         # Configurações do projeto
│   ├── __init__.py    # Indica que é um pacote Python
│   ├── settings.py    # Configurações do projeto
│   ├── urls.py        # URLs principais
│   └── wsgi.py        # Configuração para deploy
└── meuapp/             # Sua aplicação
    ├── migrations/     # Histórico de alterações do banco
    ├── __init__.py
    ├── admin.py       # Configuração do admin
    ├── apps.py        # Configuração do app
    ├── models.py      # Modelos de dados
    ├── tests.py       # Testes
    └── views.py       # Lógica de negócio
```

**Por que separar em apps?**
Apps são módulos reutilizáveis. Um e-commerce poderia ter apps separados: `produtos`, `carrinho`, `pagamentos`, `usuarios`. Isso promove organização e reuso.

## 4. Models: O Coração da Aplicação <a name="models"></a>

### O que são Models?

Models são classes Python que representam tabelas no banco de dados. Cada atributo da classe é um campo na tabela.

```python
# meuapp/models.py
from django.db import models
from django.contrib.auth.models import User

class Categoria(models.Model):
    nome = models.CharField(max_length=100)
    descricao = models.TextField(blank=True)
    data_criacao = models.DateTimeField(auto_now_add=True)
    
    class Meta:
        verbose_name_plural = "Categorias"
    
    def __str__(self):
        return self.nome

class Produto(models.Model):
    nome = models.CharField(max_length=200)
    preco = models.DecimalField(max_digits=10, decimal_places=2)
    estoque = models.IntegerField(default=0)
    categoria = models.ForeignKey(
        Categoria, 
        on_delete=models.CASCADE,
        related_name='produtos'
    )
    criado_por = models.ForeignKey(User, on_delete=models.SET_NULL, null=True)
    ativo = models.BooleanField(default=True)
    data_cadastro = models.DateTimeField(auto_now_add=True)
    data_atualizacao = models.DateTimeField(auto_now=True)
    
    def __str__(self):
        return f"{self.nome} - R${self.preco}"
    
    def verificar_estoque_baixo(self):
        return self.estoque < 10
```

### Tipos de Campos e Por Que Usá-los

- **CharField**: Texto curto (títulos, nomes)
- **TextField**: Texto longo (descrições, conteúdo)
- **IntegerField**: Números inteiros (idades, quantidades)
- **DecimalField**: Valores monetários (preços)
- **BooleanField**: Verdadeiro/Falso (ativo, status)
- **DateTimeField**: Datas e horas (cadastro, atualização)
- **ForeignKey**: Relacionamento muitos-para-um

### Explicando os parâmetros

- `max_length`: Limita o tamanho do texto (otimização de banco)
- `blank=True`: Campo opcional no formulário
- `null=True`: Permite NULL no banco de dados
- `default`: Valor padrão se não fornecido
- `auto_now_add`: Define automaticamente na criação
- `auto_now`: Atualiza automaticamente em cada save
- `on_delete=models.CASCADE`: Se categoria for deletada, produtos também são
- `related_name`: Permite acessar produtos da categoria: `categoria.produtos.all()`

### Migrações: Versionando o Banco de Dados

```bash
# Criar arquivos de migração
python manage.py makemigrations

# Ver SQL que será executado
python manage.py sqlmigrate meuapp 0001

# Aplicar migrações
python manage.py migrate
```

**Por que migrações?** 
Migrações são como "git para banco de dados". Permitem versionar, compartilhar e reverter alterações na estrutura do banco. Quando você altera um model, as migrações geram instruções SQL para atualizar o banco sem perder dados.

### Usando o ORM (Object-Relational Mapping)

O ORM permite manipular o banco usando Python, sem escrever SQL.

```python
# Criar registros
categoria = Categoria.objects.create(
    nome="Eletrônicos", 
    descricao="Produtos eletrônicos em geral"
)

# Ou criar e depois salvar
categoria = Categoria(nome="Livros")
categoria.save()

# Criar produto relacionado
produto = Produto.objects.create(
    nome="Smartphone",
    preco=1999.99,
    estoque=50,
    categoria=categoria
)

# Buscar registros
todos = Produto.objects.all()
ativos = Produto.objects.filter(ativo=True)
primeiro = Produto.objects.first()
ultimo = Produto.objects.last()
especifico = Produto.objects.get(id=1)

# Filtros avançados
caros = Produto.objects.filter(preco__gte=1000)  # maior ou igual
baratos = Produto.objects.filter(preco__lt=100)  # menor que
contem = Produto.objects.filter(nome__icontains="phone")  # contém (case insensitive)

# Relacionamentos
produtos_eletronicos = Categoria.objects.get(nome="Eletrônicos").produtos.all()

# Atualizar
produto.preco = 1799.99
produto.save()

# Deletar
produto.delete()
```

**Analogia:** O ORM é como um tradutor simultâneo - você fala Python e ele traduz para SQL, permitindo comunicação com o banco sem precisar aprender a língua dele.

## 5. Views: A Lógica de Negócio <a name="views"></a>

### Views Baseadas em Função (FBV)

```python
# meuapp/views.py
from django.shortcuts import render, get_object_or_404, redirect
from django.contrib.auth.decorators import login_required
from django.core.paginator import Paginator
from .models import Produto, Categoria
from .forms import ProdutoForm

def lista_produtos(request):
    """View que lista todos os produtos com paginação"""
    
    # Pegar parâmetros da URL
    categoria_id = request.GET.get('categoria')
    busca = request.GET.get('q')
    
    # Query base
    produtos = Produto.objects.filter(ativo=True)
    
    # Aplicar filtros se existirem
    if categoria_id:
        produtos = produtos.filter(categoria_id=categoria_id)
    
    if busca:
        produtos = produtos.filter(nome__icontains=busca)
    
    # Ordenar
    produtos = produtos.order_by('-data_cadastro')
    
    # Paginação (10 itens por página)
    paginator = Paginator(produtos, 10)
    page = request.GET.get('page')
    produtos_paginados = paginator.get_page(page)
    
    # Buscar categorias para o filtro
    categorias = Categoria.objects.all()
    
    context = {
        'produtos': produtos_paginados,
        'categorias': categorias,
        'categoria_selecionada': int(categoria_id) if categoria_id else None,
        'busca': busca,
    }
    
    return render(request, 'meuapp/lista_produtos.html', context)

@login_required
def detalhe_produto(request, produto_id):
    """View que mostra detalhes de um produto"""
    
    produto = get_object_or_404(Produto, id=produto_id, ativo=True)
    
    # Verificar se usuário pode editar
    pode_editar = request.user == produto.criado_por or request.user.is_staff
    
    context = {
        'produto': produto,
        'pode_editar': pode_editar,
    }
    
    return render(request, 'meuapp/detalhe_produto.html', context)

@login_required
def criar_produto(request):
    """View para criar novo produto"""
    
    if request.method == 'POST':
        form = ProdutoForm(request.POST, request.FILES)
        if form.is_valid():
            produto = form.save(commit=False)
            produto.criado_por = request.user
            produto.save()
            
            # Mensagem de sucesso (usando messages framework)
            from django.contrib import messages
            messages.success(request, 'Produto criado com sucesso!')
            
            return redirect('detalhe_produto', produto_id=produto.id)
    else:
        form = ProdutoForm()
    
    return render(request, 'meuapp/form_produto.html', {'form': form, 'titulo': 'Novo Produto'})
```

### Explicando cada parte

- **request**: Contém todos os dados da requisição HTTP (headers, POST data, GET params, usuário, etc.)
- **render()**: Combina template com contexto e retorna HttpResponse
- **get_object_or_404()**: Tenta buscar, se não encontrar, retorna 404
- **@login_required**: Decorator que redireciona usuários não autenticados
- **request.GET**: Parâmetros da URL (ex: ?categoria=1&q=phone)
- **request.POST**: Dados enviados via formulário
- **Paginator**: Divide resultados em páginas (melhora performance)

### Views Baseadas em Classe (CBV)

```python
# meuapp/views.py
from django.views.generic import ListView, DetailView, CreateView, UpdateView, DeleteView
from django.contrib.auth.mixins import LoginRequiredMixin, UserPassesTestMixin
from django.urls import reverse_lazy
from .models import Produto

class ProdutoListView(ListView):
    """Lista produtos usando CBV"""
    model = Produto
    template_name = 'meuapp/lista_produtos.html'
    context_object_name = 'produtos'
    paginate_by = 10
    
    def get_queryset(self):
        # Personalizar a query
        queryset = Produto.objects.filter(ativo=True)
        
        # Aplicar filtros da URL
        categoria = self.request.GET.get('categoria')
        if categoria:
            queryset = queryset.filter(categoria_id=categoria)
        
        return queryset.order_by('-data_cadastro')
    
    def get_context_data(self, **kwargs):
        # Adicionar dados extras ao contexto
        context = super().get_context_data(**kwargs)
        context['categorias'] = Categoria.objects.all()
        context['categoria_selecionada'] = self.request.GET.get('categoria')
        return context

class ProdutoDetailView(DetailView):
    """Detalhes de um produto"""
    model = Produto
    template_name = 'meuapp/detalhe_produto.html'
    
    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)
        produto = self.get_object()
        context['pode_editar'] = self.request.user == produto.criado_por or self.request.user.is_staff
        return context

class ProdutoCreateView(LoginRequiredMixin, CreateView):
    """Cria novo produto"""
    model = Produto
    fields = ['nome', 'preco', 'estoque', 'categoria', 'descricao']
    template_name = 'meuapp/form_produto.html'
    success_url = reverse_lazy('lista_produtos')
    
    def form_valid(self, form):
        form.instance.criado_por = self.request.user
        return super().form_valid(form)
    
    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)
        context['titulo'] = 'Novo Produto'
        return context

class ProdutoUpdateView(LoginRequiredMixin, UserPassesTestMixin, UpdateView):
    """Edita produto"""
    model = Produto
    fields = ['nome', 'preco', 'estoque', 'categoria', 'descricao']
    template_name = 'meuapp/form_produto.html'
    
    def test_func(self):
        # Verifica se usuário pode editar
        produto = self.get_object()
        return self.request.user == produto.criado_por or self.request.user.is_staff
    
    def get_success_url(self):
        return reverse_lazy('detalhe_produto', kwargs={'pk': self.object.pk})
    
    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)
        context['titulo'] = f'Editando: {self.object.nome}'
        return context
```

**FBV vs CBV: Qual usar?**
- **FBV**: Mais explícitas, fáceis de entender, boas para lógica complexa
- **CBV**: Menos código, reutilizáveis, seguem padrões (DRY)

Escolha baseado na complexidade e preferência da equipe.

## 6. Templates: A Camada de Apresentação <a name="templates"></a>

### Estrutura de Templates

```
meuapp/templates/
├── base.html              # Template base
├── meuapp/
│   ├── lista_produtos.html
│   ├── detalhe_produto.html
│   └── form_produto.html
└── includes/
    ├── header.html
    └── footer.html
```

### Template Base

```html
<!-- templates/base.html -->
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{% block title %}Meu E-commerce{% endblock %}</title>
    
    <!-- Bootstrap CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet">
    
    {% block extra_css %}{% endblock %}
</head>
<body>
    <!-- Header -->
    {% include 'includes/header.html' %}
    
    <!-- Mensagens -->
    {% if messages %}
        {% for message in messages %}
            <div class="alert alert-{{ message.tags }} alert-dismissible fade show" role="alert">
                {{ message }}
                <button type="button" class="btn-close" data-bs-dismiss="alert"></button>
            </div>
        {% endfor %}
    {% endif %}
    
    <!-- Conteúdo Principal -->
    <main class="container my-4">
        {% block content %}
        {% endblock %}
    </main>
    
    <!-- Footer -->
    {% include 'includes/footer.html' %}
    
    <!-- Bootstrap JS -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/js/bootstrap.bundle.min.js"></script>
    
    {% block extra_js %}{% endblock %}
</body>
</html>
```

### Template de Lista

```html
<!-- templates/meuapp/lista_produtos.html -->
{% extends 'base.html' %}

{% block title %}Produtos - Meu E-commerce{% endblock %}

{% block content %}
<div class="row">
    <!-- Filtros -->
    <div class="col-md-3">
        <div class="card">
            <div class="card-header">
                <h5>Filtros</h5>
            </div>
            <div class="card-body">
                <form method="get">
                    <!-- Busca -->
                    <div class="mb-3">
                        <label class="form-label">Buscar produto</label>
                        <input type="text" name="q" class="form-control" 
                               value="{{ busca|default:'' }}" 
                               placeholder="Digite para buscar...">
                    </div>
                    
                    <!-- Categorias -->
                    <div class="mb-3">
                        <label class="form-label">Categoria</label>
                        <select name="categoria" class="form-select">
                            <option value="">Todas</option>
                            {% for categoria in categorias %}
                                <option value="{{ categoria.id }}" 
                                    {% if categoria_selecionada == categoria.id %}selected{% endif %}>
                                    {{ categoria.nome }}
                                </option>
                            {% endfor %}
                        </select>
                    </div>
                    
                    <button type="submit" class="btn btn-primary w-100">
                        Aplicar Filtros
                    </button>
                </form>
            </div>
        </div>
    </div>
    
    <!-- Lista de Produtos -->
    <div class="col-md-9">
        <div class="d-flex justify-content-between align-items-center mb-4">
            <h2>Produtos</h2>
            {% if user.is_authenticated %}
                <a href="{% url 'criar_produto' %}" class="btn btn-success">
                    Novo Produto
                </a>
            {% endif %}
        </div>
        
        <div class="row">
            {% for produto in produtos %}
                <div class="col-md-4 mb-4">
                    <div class="card h-100">
                        <div class="card-body">
                            <h5 class="card-title">{{ produto.nome }}</h5>
                            <p class="card-text">
                                <strong>Preço:</strong> R$ {{ produto.preco }}<br>
                                <strong>Estoque:</strong> 
                                <span class="{% if produto.estoque < 10 %}text-danger{% endif %}">
                                    {{ produto.estoque }} unidades
                                </span><br>
                                <strong>Categoria:</strong> {{ produto.categoria.nome }}
                            </p>
                            <a href="{% url 'detalhe_produto' produto.id %}" 
                               class="btn btn-primary">Ver Detalhes</a>
                        </div>
                    </div>
                </div>
            {% empty %}
                <div class="col-12">
                    <div class="alert alert-info">
                        Nenhum produto encontrado.
                    </div>
                </div>
            {% endfor %}
        </div>
        
        <!-- Paginação -->
        {% if produtos.has_other_pages %}
            <nav aria-label="Paginação">
                <ul class="pagination justify-content-center">
                    {% if produtos.has_previous %}
                        <li class="page-item">
                            <a class="page-link" href="?page=1{% if busca %}&q={{ busca }}{% endif %}{% if categoria_selecionada %}&categoria={{ categoria_selecionada }}{% endif %}">
                                &laquo; Primeira
                            </a>
                        </li>
                        <li class="page-item">
                            <a class="page-link" href="?page={{ produtos.previous_page_number }}{% if busca %}&q={{ busca }}{% endif %}{% if categoria_selecionada %}&categoria={{ categoria_selecionada }}{% endif %}">
                                Anterior
                            </a>
                        </li>
                    {% endif %}
                    
                    <li class="page-item active">
                        <span class="page-link">
                            Página {{ produtos.number }} de {{ produtos.paginator.num_pages }}
                        </span>
                    </li>
                    
                    {% if produtos.has_next %}
                        <li class="page-item">
                            <a class="page-link" href="?page={{ produtos.next_page_number }}{% if busca %}&q={{ busca }}{% endif %}{% if categoria_selecionada %}&categoria={{ categoria_selecionada }}{% endif %}">
                                Próxima
                            </a>
                        </li>
                        <li class="page-item">
                            <a class="page-link" href="?page={{ produtos.paginator.num_pages }}{% if busca %}&q={{ busca }}{% endif %}{% if categoria_selecionada %}&categoria={{ categoria_selecionada }}{% endif %}">
                                Última &raquo;
                            </a>
                        </li>
                    {% endif %}
                </ul>
            </nav>
        {% endif %}
    </div>
</div>
{% endblock %}
```

### Template de Detalhe

```html
<!-- templates/meuapp/detalhe_produto.html -->
{% extends 'base.html' %}

{% block title %}{{ produto.nome }} - Meu E-commerce{% endblock %}

{% block content %}
<div class="row">
    <div class="col-md-8 offset-md-2">
        <div class="card">
            <div class="card-header d-flex justify-content-between align-items-center">
                <h3>{{ produto.nome }}</h3>
                {% if pode_editar %}
                    <div>
                        <a href="{% url 'editar_produto' produto.id %}" class="btn btn-warning btn-sm">
                            Editar
                        </a>
                        <button type="button" class="btn btn-danger btn-sm" data-bs-toggle="modal" data-bs-target="#deleteModal">
                            Excluir
                        </button>
                    </div>
                {% endif %}
            </div>
            <div class="card-body">
                <dl class="row">
                    <dt class="col-sm-3">Preço</dt>
                    <dd class="col-sm-9">R$ {{ produto.preco }}</dd>
                    
                    <dt class="col-sm-3">Estoque</dt>
                    <dd class="col-sm-9">
                        <span class="badge {% if produto.estoque < 10 %}bg-danger{% else %}bg-success{% endif %}">
                            {{ produto.estoque }} unidades
                        </span>
                    </dd>
                    
                    <dt class="col-sm-3">Categoria</dt>
                    <dd class="col-sm-9">{{ produto.categoria.nome }}</dd>
                    
                    <dt class="col-sm-3">Descrição</dt>
                    <dd class="col-sm-9">{{ produto.descricao|linebreaks }}</dd>
                    
                    <dt class="col-sm-3">Cadastrado por</dt>
                    <dd class="col-sm-9">{{ produto.criado_por.get_full_name|default:produto.criado_por.username }}</dd>
                    
                    <dt class="col-sm-3">Data de cadastro</dt>
                    <dd class="col-sm-9">{{ produto.data_cadastro|date:"d/m/Y H:i" }}</dd>
                    
                    <dt class="col-sm-3">Última atualização</dt>
                    <dd class="col-sm-9">{{ produto.data_atualizacao|date:"d/m/Y H:i" }}</dd>
                </dl>
            </div>
            <div class="card-footer">
                <a href="{% url 'lista_produtos' %}" class="btn btn-secondary">
                    Voltar para lista
                </a>
            </div>
        </div>
    </div>
</div>

<!-- Modal de Confirmação de Exclusão -->
{% if pode_editar %}
<div class="modal fade" id="deleteModal" tabindex="-1">
    <div class="modal-dialog">
        <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title">Confirmar exclusão</h5>
                <button type="button" class="btn-close" data-bs-dismiss="modal"></button>
            </div>
            <div class="modal-body">
                <p>Tem certeza que deseja excluir o produto "{{ produto.nome }}"?</p>
                <p class="text-danger"><small>Esta ação não pode ser desfeita.</small></p>
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cancelar</button>
                <form action="{% url 'deletar_produto' produto.id %}" method="post" style="display: inline;">
                    {% csrf_token %}
                    <button type="submit" class="btn btn-danger">Confirmar exclusão</button>
                </form>
            </div>
        </div>
    </div>
</div>
{% endif %}
{% endblock %}
```

### Template Tags e Filtros

```html
<!-- Exemplos de template tags e filtros -->

<!-- Variáveis -->
{{ produto.nome }}

<!-- Filtros -->
{{ produto.preco|floatformat:2 }}
{{ produto.descricao|truncatewords:30 }}
{{ produto.data_cadastro|date:"d/m/Y" }}
{{ usuario.email|default:"Sem email" }}

<!-- Tags de controle -->
{% if user.is_authenticated %}
    <p>Bem-vindo, {{ user.username }}!</p>
{% elif user.is_anonymous %}
    <p>Faça login para continuar.</p>
{% endif %}

<!-- Loops -->
{% for produto in produtos %}
    <li>{{ forloop.counter }} - {{ produto.nome }}</li>
{% empty %}
    <li>Nenhum produto encontrado</li>
{% endfor %}

<!-- URLs -->
<a href="{% url 'detalhe_produto' produto.id %}">Ver detalhes</a>

<!-- Carregar arquivos estáticos -->
{% load static %}
<img src="{% static 'images/logo.png' %}" alt="Logo">

<!-- CSRF Token (para formulários) -->
{% csrf_token %}

<!-- Comentários (não renderizados) -->
{# Este é um comentário que não aparece no HTML #}
```

**Por que a sintaxe {{ }} e {% %}?**
- **{{ }}** para variáveis: "mostre este valor aqui"
- **{% %}** para lógica: "execute esta instrução"

Isso mantém a separação entre lógica (Python) e apresentação (HTML).

## 7. URLs: O Roteamento <a name="urls"></a>

### Configuração de URLs

```python
# meuprojeto/urls.py (URLs principais)
from django.contrib import admin
from django.urls import path, include
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('meuapp.urls')),  # Inclui URLs do app
]

# Servir arquivos de mídia em desenvolvimento
if settings.DEBUG:
    urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
    urlpatterns += static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
```

```python
# meuapp/urls.py (URLs do app)
from django.urls import path
from . import views

# Nomeando o app para namespaces
app_name = 'meuapp'

urlpatterns = [
    # URLs baseadas em função
    path('', views.lista_produtos, name='lista_produtos'),
    path('produto/<int:produto_id>/', views.detalhe_produto, name='detalhe_produto'),
    path('produto/novo/', views.criar_produto, name='criar_produto'),
    
    # URLs baseadas em classe
    # path('', views.ProdutoListView.as_view(), name='lista_produtos'),
    # path('produto/<int:pk>/', views.ProdutoDetailView.as_view(), name='detalhe_produto'),
    # path('produto/novo/', views.ProdutoCreateView.as_view(), name='criar_produto'),
    # path('produto/<int:pk>/editar/', views.ProdutoUpdateView.as_view(), name='editar_produto'),
]
```

### Entendendo os padrões de URL

- `''`: URL raiz (ex: /)
- `'admin/'`: URLs começando com admin/
- `'produto/<int:produto_id>/'`: Captura um inteiro e passa como parâmetro
- `name='detalhe_produto'`: Nome único para referenciar a URL

**Conversores de path:**
- `str`: Qualquer string (sem barras)
- `int`: Números inteiros
- `slug`: String com hífens e underscores (ex: "meu-produto")
- `uuid`: UUID
- `path`: Qualquer string incluindo barras

## 8. Forms: Manipulação de Dados <a name="forms"></a>

### Forms vs ModelForms

```python
# meuapp/forms.py
from django import forms
from django.core.exceptions import ValidationError
from .models import Produto, Categoria

# Form baseado em modelo (mais comum)
class ProdutoForm(forms.ModelForm):
    class Meta:
        model = Produto
        fields = ['nome', 'preco', 'estoque', 'categoria', 'descricao']
        widgets = {
            'descricao': forms.Textarea(attrs={'rows': 4, 'class': 'form-control'}),
            'nome': forms.TextInput(attrs={'class': 'form-control', 'placeholder': 'Digite o nome do produto'}),
            'preco': forms.NumberInput(attrs={'class': 'form-control', 'step': '0.01'}),
            'estoque': forms.NumberInput(attrs={'class': 'form-control'}),
            'categoria': forms.Select(attrs={'class': 'form-select'}),
        }
        labels = {
            'nome': 'Nome do Produto',
            'preco': 'Preço (R$)',
            'estoque': 'Quantidade em Estoque',
        }
        help_texts = {
            'preco': 'Use ponto para separar centavos (ex: 99.90)',
        }
    
    def clean_preco(self):
        """Validação personalizada para o preço"""
        preco = self.cleaned_data.get('preco')
        if preco <= 0:
            raise ValidationError('O preço deve ser maior que zero')
        return preco
    
    def clean_estoque(self):
        """Validação personalizada para o estoque"""
        estoque = self.cleaned_data.get('estoque')
        if estoque < 0:
            raise ValidationError('O estoque não pode ser negativo')
        return estoque
    
    def clean(self):
        """Validação que envolve múltiplos campos"""
        cleaned_data = super().clean()
        preco = cleaned_data.get('preco')
        estoque = cleaned_data.get('estoque')
        
        # Exemplo: desconto automático se estoque alto
        if estoque and estoque > 100 and preco:
            cleaned_data['preco'] = preco * 0.9  # 10% de desconto
        
        return cleaned_data

# Formulário personalizado (não baseado em modelo)
class ContatoForm(forms.Form):
    nome = forms.CharField(
        max_length=100,
        widget=forms.TextInput(attrs={'class': 'form-control'}),
        error_messages={'required': 'Por favor, informe seu nome'}
    )
    email = forms.EmailField(
        widget=forms.EmailInput(attrs={'class': 'form-control'}),
        error_messages={'invalid': 'Por favor, informe um email válido'}
    )
    mensagem = forms.CharField(
        widget=forms.Textarea(attrs={'class': 'form-control', 'rows': 5}),
        max_length=1000
    )
    
    def clean_email(self):
        """Validação personalizada do email"""
        email = self.cleaned_data.get('email')
        if not email.endswith('@gmail.com') and not email.endswith('@hotmail.com'):
            raise ValidationError('Por favor, use um email do Gmail ou Hotmail')
        return email

# Formulário de busca
class BuscaForm(forms.Form):
    q = forms.CharField(
        required=False,
        widget=forms.TextInput(attrs={'class': 'form-control', 'placeholder': 'Buscar produtos...'})
    )
    categoria = forms.ModelChoiceField(
        queryset=Categoria.objects.all(),
        required=False,
        empty_label="Todas as categorias",
        widget=forms.Select(attrs={'class': 'form-select'})
    )
    preco_min = forms.DecimalField(
        required=False,
        widget=forms.NumberInput(attrs={'class': 'form-control', 'placeholder': 'Preço mínimo'})
    )
    preco_max = forms.DecimalField(
        required=False,
        widget=forms.NumberInput(attrs={'class': 'form-control', 'placeholder': 'Preço máximo'})
    )
```

### Template do Formulário

```html
<!-- templates/meuapp/form_produto.html -->
{% extends 'base.html' %}

{% block title %}{{ titulo }} - Meu E-commerce{% endblock %}

{% block content %}
<div class="row">
    <div class="col-md-8 offset-md-2">
        <div class="card">
            <div class="card-header">
                <h3>{{ titulo }}</h3>
            </div>
            <div class="card-body">
                <form method="post" enctype="multipart/form-data">
                    {% csrf_token %}
                    
                    <!-- Exibir erros não-campo (gerais) -->
                    {% if form.non_field_errors %}
                        <div class="alert alert-danger">
                            {% for error in form.non_field_errors %}
                                {{ error }}
                            {% endfor %}
                        </div>
                    {% endif %}
                    
                    <!-- Campos do formulário -->
                    {% for field in form %}
                        <div class="mb-3">
                            <label for="{{ field.id_for_label }}" class="form-label">
                                {{ field.label }}
                                {% if field.field.required %}
                                    <span class="text-danger">*</span>
                                {% endif %}
                            </label>
                            
                            {{ field }}
                            
                            <!-- Help text -->
                            {% if field.help_text %}
                                <div class="form-text">{{ field.help_text }}</div>
                            {% endif %}
                            
                            <!-- Erros do campo -->
                            {% if field.errors %}
                                <div class="invalid-feedback d-block">
                                    {% for error in field.errors %}
                                        {{ error }}
                                    {% endfor %}
                                </div>
                            {% endif %}
                        </div>
                    {% endfor %}
                    
                    <div class="d-grid gap-2 d-md-flex justify-content-md-end">
                        <a href="{% url 'meuapp:lista_produtos' %}" class="btn btn-secondary me-md-2">
                            Cancelar
                        </a>
                        <button type="submit" class="btn btn-primary">
                            {% if produto %}Atualizar{% else %}Criar{% endif %} Produto
                        </button>
                    </div>
                </form>
            </div>
        </div>
    </div>
</div>
{% endblock %}
```

### Processando o Formulário na View

```python
# Exemplo de view processando formulário
def contato(request):
    if request.method == 'POST':
        form = ContatoForm(request.POST)
        if form.is_valid():
            # Dados limpos e validados
            nome = form.cleaned_data['nome']
            email = form.cleaned_data['email']
            mensagem = form.cleaned_data['mensagem']
            
            # Fazer algo com os dados (enviar email, salvar no banco, etc.)
            # send_mail(...)
            
            messages.success(request, f'Obrigado {nome}! Sua mensagem foi enviada.')
            return redirect('contato_sucesso')
    else:
        form = ContatoForm()
    
    return render(request, 'meuapp/contato.html', {'form': form})
```

**Por que forms são importantes?**
- **Segurança**: Proteção contra CSRF, sanitização de dados
- **Validação**: Garante que dados estejam no formato correto
- **Reutilização**: Mesmo form pode ser usado em múltiplas views
- **Renderização**: Gera HTML automaticamente

## 9. Admin: Interface Administrativa <a name="admin"></a>

### Configurando o Admin

```python
# meuapp/admin.py
from django.contrib import admin
from django.utils.html import format_html
from .models import Categoria, Produto

@admin.register(Categoria)
class CategoriaAdmin(admin.ModelAdmin):
    list_display = ['nome', 'qtd_produtos', 'data_criacao']
    search_fields = ['nome']
    list_filter = ['data_criacao']
    
    def qtd_produtos(self, obj):
        return obj.produtos.count()
    qtd_produtos.short_description = 'Quantidade de Produtos'

@admin.register(Produto)
class ProdutoAdmin(admin.ModelAdmin):
    # Campos a exibir na lista
    list_display = [
        'nome', 
        'preco', 
        'estoque_colorido', 
        'categoria', 
        'criado_por',
        'ativo',
        'data_cadastro'
    ]
    
    # Campos que são links
    list_display_links = ['nome']
    
    # Campos que podem ser editados diretamente na lista
    list_editable = ['preco', 'ativo']
    
    # Filtros laterais
    list_filter = ['ativo', 'categoria', 'data_cadastro']
    
    # Campos de busca
    search_fields = ['nome', 'descricao']
    
    # Paginação
    list_per_page = 25
    
    # Ordenação padrão
    ordering = ['-data_cadastro']
    
    # Campos somente leitura
    readonly_fields = ['data_cadastro', 'data_atualizacao']
    
    # Organização dos campos no formulário
    fieldsets = (
        ('Informações Básicas', {
            'fields': ('nome', 'preco', 'estoque', 'categoria')
        }),
        ('Descrição', {
            'fields': ('descricao',),
            'classes': ('wide',)
        }),
        ('Status', {
            'fields': ('ativo', 'criado_por', 'data_cadastro', 'data_atualizacao'),
            'classes': ('collapse',)  # Seção recolhível
        }),
    )
    
    # Ações em massa
    actions = ['ativar_produtos', 'desativar_produtos']
    
    def estoque_colorido(self, obj):
        """Exibe estoque com cor baseada na quantidade"""
        if obj.estoque < 10:
            return format_html('<span style="color: red; font-weight: bold;">{}</span>', obj.estoque)
        elif obj.estoque < 50:
            return format_html('<span style="color: orange;">{}</span>', obj.estoque)
        return obj.estoque
    estoque_colorido.short_description = 'Estoque'
    
    def ativar_produtos(self, request, queryset):
        """Ação para ativar produtos selecionados"""
        atualizados = queryset.update(ativo=True)
        self.message_user(
            request, 
            f'{atualizados} produto(s) foram ativados com sucesso.'
        )
    ativar_produtos.short_description = "Ativar produtos selecionados"
    
    def desativar_produtos(self, request, queryset):
        """Ação para desativar produtos selecionados"""
        atualizados = queryset.update(ativo=False)
        self.message_user(
            request, 
            f'{atualizados} produto(s) foram desativados com sucesso.'
        )
    desativar_produtos.short_description = "Desativar produtos selecionados"
    
    def save_model(self, request, obj, form, change):
        """Sobrescrever save para definir criador"""
        if not change:  # Se é criação
            obj.criado_por = request.user
        super().save_model(request, obj, form, change)
    
    def get_queryset(self, request):
        """Otimizar queries com select_related"""
        return super().get_queryset(request).select_related('categoria', 'criado_por')
```

**Por que usar o Admin?**
- Interface pronta para CRUD (Create, Read, Update, Delete)
- Controle de acesso integrado
- Personalizável para necessidades específicas
- Perfeito para equipes internas gerenciarem conteúdo

## 10. Autenticação e Autorização <a name="auth"></a>

### Sistema de Usuários

```python
# models.py - Estendendo o modelo User
from django.contrib.auth.models import User
from django.db import models

class Perfil(models.Model):
    """Extensão do modelo User para dados adicionais"""
    user = models.OneToOneField(User, on_delete=models.CASCADE, related_name='perfil')
    telefone = models.CharField(max_length=20, blank=True)
    data_nascimento = models.DateField(null=True, blank=True)
    cpf = models.CharField(max_length=14, unique=True, blank=True)
    avatar = models.ImageField(upload_to='avatares/', null=True, blank=True)
    tipo_usuario = models.CharField(
        max_length=20,
        choices=[
            ('cliente', 'Cliente'),
            ('vendedor', 'Vendedor'),
            ('gerente', 'Gerente'),
        ],
        default='cliente'
    )
    receber_ofertas = models.BooleanField(default=True)
    
    def __str__(self):
        return f"Perfil de {self.user.username}"
    
    @property
    def nome_completo(self):
        return self.user.get_full_name() or self.user.username

# signals.py - Criar perfil automaticamente
from django.db.models.signals import post_save
from django.dispatch import receiver
from django.contrib.auth.models import User

@receiver(post_save, sender=User)
def criar_perfil_usuario(sender, instance, created, **kwargs):
    """Cria perfil automaticamente quando usuário é criado"""
    if created:
        Perfil.objects.create(user=instance)

@receiver(post_save, sender=User)
def salvar_perfil_usuario(sender, instance, **kwargs):
    """Salva perfil quando usuário é salvo"""
    instance.perfil.save()
```

### Views de Autenticação

```python
# views.py
from django.contrib.auth import authenticate, login, logout
from django.contrib.auth.forms import UserCreationForm, AuthenticationForm
from django.contrib.auth.decorators import login_required
from django.shortcuts import render, redirect
from django.contrib import messages

def registrar_usuario(request):
    """View de registro de usuário"""
    if request.method == 'POST':
        form = UserCreationForm(request.POST)
        if form.is_valid():
            user = form.save()
            
            # Fazer login automático após registro
            login(request, user)
            
            messages.success(request, 'Registro realizado com sucesso!')
            return redirect('meuapp:lista_produtos')
    else:
        form = UserCreationForm()
    
    return render(request, 'registration/registro.html', {'form': form})

def login_usuario(request):
    """View personalizada de login"""
    if request.method == 'POST':
        form = AuthenticationForm(request, data=request.POST)
        if form.is_valid():
            username = form.cleaned_data.get('username')
            password = form.cleaned_data.get('password')
            user = authenticate(username=username, password=password)
            
            if user is not None:
                login(request, user)
                
                # Redirecionar para página anterior ou padrão
                next_url = request.GET.get('next', 'meuapp:lista_produtos')
                return redirect(next_url)
    else:
        form = AuthenticationForm()
    
    return render(request, 'registration/login.html', {'form': form})

@login_required
def logout_usuario(request):
    """View de logout"""
    logout(request)
    messages.info(request, 'Você saiu do sistema.')
    return redirect('meuapp:lista_produtos')

@login_required
def perfil_usuario(request):
    """View do perfil do usuário"""
    return render(request, 'registration/perfil.html', {'perfil': request.user.perfil})
```

### Templates de Autenticação

```html
<!-- templates/registration/login.html -->
{% extends 'base.html' %}

{% block title %}Login - Meu E-commerce{% endblock %}

{% block content %}
<div class="row">
    <div class="col-md-6 offset-md-3">
        <div class="card">
            <div class="card-header">
                <h3>Login</h3>
            </div>
            <div class="card-body">
                <form method="post">
                    {% csrf_token %}
                    
                    {% if form.errors %}
                        <div class="alert alert-danger">
                            Usuário ou senha incorretos.
                        </div>
                    {% endif %}
                    
                    <div class="mb-3">
                        <label for="id_username" class="form-label">Usuário</label>
                        <input type="text" name="username" class="form-control" id="id_username" required>
                    </div>
                    
                    <div class="mb-3">
                        <label for="id_password" class="form-label">Senha</label>
                        <input type="password" name="password" class="form-control" id="id_password" required>
                    </div>
                    
                    {% if request.GET.next %}
                        <input type="hidden" name="next" value="{{ request.GET.next }}">
                    {% endif %}
                    
                    <button type="submit" class="btn btn-primary w-100">Entrar</button>
                </form>
                
                <hr>
                
                <p class="text-center">
                    Não tem uma conta? 
                    <a href="{% url 'registrar' %}">Registre-se aqui</a>
                </p>
            </div>
        </div>
    </div>
</div>
{% endblock %}
```

### Controle de Acesso

```python
# Decorators para views baseadas em função
from django.contrib.auth.decorators import login_required, user_passes_test

def grupo_required(grupo_nome):
    """Decorator personalizado para verificar grupo"""
    def decorator(view_func):
        @login_required
        def _wrapped_view(request, *args, **kwargs):
            if request.user.groups.filter(name=grupo_nome).exists():
                return view_func(request, *args, **kwargs)
            messages.error(request, 'Você não tem permissão para acessar esta página.')
            return redirect('meuapp:lista_produtos')
        return _wrapped_view
    return decorator

@grupo_required('vendedores')
def painel_vendedor(request):
    """Apenas vendedores podem acessar"""
    return render(request, 'meuapp/painel_vendedor.html')

# Mixins para views baseadas em classe
from django.contrib.auth.mixins import LoginRequiredMixin, PermissionRequiredMixin

class PainelGerenteView(LoginRequiredMixin, PermissionRequiredMixin, TemplateView):
    template_name = 'meuapp/painel_gerente.html'
    permission_required = 'meuapp.pode_ver_relatorios'
    login_url = '/login/'
    
    def handle_no_permission(self):
        messages.error(self.request, 'Acesso negado.')
        return redirect('meuapp:lista_produtos')
```

## 11. APIs com Django REST Framework <a name="api"></a>

### Instalação e Configuração

```bash
pip install djangorestframework
pip install django-cors-headers  # Para permitir requisições de outros domínios
```

```python
# settings.py
INSTALLED_APPS = [
    # ...
    'rest_framework',
    'corsheaders',
]

MIDDLEWARE = [
    'corsheaders.middleware.CorsMiddleware',  # Deve vir primeiro
    # ...
]

# Configurações do REST Framework
REST_FRAMEWORK = {
    'DEFAULT_PERMISSION_CLASSES': [
        'rest_framework.permissions.IsAuthenticatedOrReadOnly',
    ],
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'rest_framework.authentication.SessionAuthentication',
        'rest_framework.authentication.TokenAuthentication',
    ],
    'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.PageNumberPagination',
    'PAGE_SIZE': 20,
    'DEFAULT_FILTER_BACKENDS': [
        'rest_framework.filters.SearchFilter',
        'rest_framework.filters.OrderingFilter',
    ],
}

# CORS (Cross-Origin Resource Sharing)
CORS_ALLOWED_ORIGINS = [
    "http://localhost:3000",  # React app
    "http://127.0.0.1:3000",
]
```

### Serializers

```python
# api/serializers.py
from rest_framework import serializers
from meuapp.models import Produto, Categoria
from django.contrib.auth.models import User

class CategoriaSerializer(serializers.ModelSerializer):
    """Serializer para Categoria"""
    class Meta:
        model = Categoria
        fields = ['id', 'nome', 'descricao', 'data_criacao']

class ProdutoSerializer(serializers.ModelSerializer):
    """Serializer para Produto"""
    categoria_nome = serializers.CharField(source='categoria.nome', read_only=True)
    criado_por_username = serializers.CharField(source='criado_por.username', read_only=True)
    estoque_baixo = serializers.BooleanField(read_only=True)
    
    class Meta:
        model = Produto
        fields = [
            'id', 'nome', 'preco', 'estoque', 'categoria', 
            'categoria_nome', 'descricao', 'criado_por', 
            'criado_por_username', 'ativo', 'data_cadastro',
            'data_atualizacao', 'estoque_baixo'
        ]
        read_only_fields = ['criado_por', 'data_cadastro', 'data_atualizacao']
    
    def validate_preco(self, value):
        """Validação personalizada"""
        if value <= 0:
            raise serializers.ValidationError("O preço deve ser maior que zero")
        return value
    
    def create(self, validated_data):
        """Criar produto com usuário atual"""
        validated_data['criado_por'] = self.context['request'].user
        return super().create(validated_data)

class UserSerializer(serializers.ModelSerializer):
    """Serializer para Usuário"""
    class Meta:
        model = User
        fields = ['id', 'username', 'email', 'first_name', 'last_name']
```

### Views da API

```python
# api/views.py
from rest_framework import generics, viewsets, filters, status
from rest_framework.decorators import api_view, action
from rest_framework.response import Response
from rest_framework.permissions import IsAuthenticatedOrReadOnly, IsAuthenticated
from rest_framework.pagination import PageNumberPagination
from django_filters.rest_framework import DjangoFilterBackend
from meuapp.models import Produto, Categoria
from .serializers import ProdutoSerializer, CategoriaSerializer, UserSerializer

# ViewSet (mais completo, gera todas as operações CRUD)
class ProdutoViewSet(viewsets.ModelViewSet):
    """
    ViewSet completo para Produto.
    Fornece list, create, retrieve, update, partial_update, destroy.
    """
    queryset = Produto.objects.filter(ativo=True).select_related('categoria', 'criado_por')
    serializer_class = ProdutoSerializer
    permission_classes = [IsAuthenticatedOrReadOnly]
    
    # Filtros
    filter_backends = [DjangoFilterBackend, filters.SearchFilter, filters.OrderingFilter]
    filterset_fields = ['categoria', 'ativo']
    search_fields = ['nome', 'descricao']
    ordering_fields = ['preco', 'data_cadastro', 'nome']
    ordering = ['-data_cadastro']
    
    def perform_create(self, serializer):
        """Associa o usuário atual ao criar produto"""
        serializer.save(criado_por=self.request.user)
    
    @action(detail=True, methods=['post'])
    def desativar(self, request, pk=None):
        """Ação personalizada para desativar produto"""
        produto = self.get_object()
        produto.ativo = False
        produto.save()
        return Response({'status': 'produto desativado'})
    
    @action(detail=False, methods=['get'])
    def estoque_baixo(self, request):
        """Lista produtos com estoque baixo"""
        produtos = self.get_queryset().filter(estoque__lt=10)
        page = self.paginate_queryset(produtos)
        if page is not None:
            serializer = self.get_serializer(page, many=True)
            return self.get_paginated_response(serializer.data)
        serializer = self.get_serializer(produtos, many=True)
        return Response(serializer.data)

# API baseada em classes genéricas (mais simples)
class CategoriaListCreateView(generics.ListCreateAPIView):
    """Lista e cria categorias"""
    queryset = Categoria.objects.all()
    serializer_class = CategoriaSerializer
    permission_classes = [IsAuthenticatedOrReadOnly]

class CategoriaRetrieveUpdateDestroyView(generics.RetrieveUpdateDestroyAPIView):
    """Recupera, atualiza e deleta uma categoria"""
    queryset = Categoria.objects.all()
    serializer_class = CategoriaSerializer
    permission_classes = [IsAuthenticated]

# API baseada em função (@api_view)
@api_view(['GET', 'POST'])
def estatisticas(request):
    """Endpoint de estatísticas"""
    if request.method == 'GET':
        data = {
            'total_produtos': Produto.objects.count(),
            'total_categorias': Categoria.objects.count(),
            'produtos_ativos': Produto.objects.filter(ativo=True).count(),
            'valor_total_estoque': Produto.objects.aggregate(
                total=models.Sum(models.F('preco') * models.F('estoque'))
            )['total'] or 0,
        }
        return Response(data)
    elif request.method == 'POST':
        # Pode ser usado para gerar relatórios
        return Response({'status': 'relatório gerado'}, status=status.HTTP_201_CREATED)
```

### URLs da API

```python
# api/urls.py
from django.urls import path, include
from rest_framework.routers import DefaultRouter
from rest_framework.authtoken.views import obtain_auth_token
from . import views

# Router para ViewSets
router = DefaultRouter()
router.register(r'produtos', views.ProdutoViewSet, basename='produto')

urlpatterns = [
    # URLs do router
    path('', include(router.urls)),
    
    # URLs manuais
    path('categorias/', views.CategoriaListCreateView.as_view(), name='categoria-list'),
    path('categorias/<int:pk>/', views.CategoriaRetrieveUpdateDestroyView.as_view(), name='categoria-detail'),
    
    # Estatísticas
    path('estatisticas/', views.estatisticas, name='estatisticas'),
    
    # Autenticação
    path('auth/', include('rest_framework.urls')),
    path('auth/token/', obtain_auth_token, name='api_token_auth'),
]
```

### Testando a API

```python
# Usando curl
curl -X GET http://localhost:8000/api/produtos/
curl -X POST http://localhost:8000/api/produtos/ -H "Content-Type: application/json" -d '{"nome":"Teste","preco":99.90,"estoque":10}'

# Usando Python requests
import requests
response = requests.get('http://localhost:8000/api/produtos/')
print(response.json())

# Com autenticação
token = 'seu_token_aqui'
headers = {'Authorization': f'Token {token}'}
response = requests.post(
    'http://localhost:8000/api/produtos/',
    json={'nome': 'Novo Produto', 'preco': 199.90, 'estoque': 20},
    headers=headers
)
```

## 12. Exemplo Prático Completo <a name="exemplo"></a>

### Sistema de Blog com Django

Vamos construir um blog completo para demonstrar todos os conceitos.

### Modelos

```python
# blog/models.py
from django.db import models
from django.contrib.auth.models import User
from django.utils.text import slugify
from django.urls import reverse

class Categoria(models.Model):
    nome = models.CharField(max_length=100, unique=True)
    slug = models.SlugField(max_length=100, unique=True, blank=True)
    descricao = models.TextField(blank=True)
    
    class Meta:
        verbose_name_plural = "Categorias"
    
    def save(self, *args, **kwargs):
        if not self.slug:
            self.slug = slugify(self.nome)
        super().save(*args, **kwargs)
    
    def __str__(self):
        return self.nome

class Post(models.Model):
    titulo = models.CharField(max_length=200)
    slug = models.SlugField(max_length=200, unique=True, blank=True)
    conteudo = models.TextField()
    resumo = models.TextField(max_length=300, blank=True)
    autor = models.ForeignKey(User, on_delete=models.CASCADE, related_name='posts')
    categorias = models.ManyToManyField(Categoria, related_name='posts')
    imagem = models.ImageField(upload_to='blog/', null=True, blank=True)
    
    # Status
    STATUS_CHOICES = [
        ('rascunho', 'Rascunho'),
        ('publicado', 'Publicado'),
        ('arquivado', 'Arquivado'),
    ]
    status = models.CharField(max_length=20, choices=STATUS_CHOICES, default='rascunho')
    
    # Controle
    views = models.IntegerField(default=0)
    publicado_em = models.DateTimeField(null=True, blank=True)
    criado_em = models.DateTimeField(auto_now_add=True)
    atualizado_em = models.DateTimeField(auto_now=True)
    
    class Meta:
        ordering = ['-publicado_em', '-criado_em']
    
    def save(self, *args, **kwargs):
        if not self.slug:
            self.slug = slugify(self.titulo)
        super().save(*args, **kwargs)
    
    def get_absolute_url(self):
        return reverse('blog:detalhe_post', kwargs={'slug': self.slug})
    
    def __str__(self):
        return self.titulo

class Comentario(models.Model):
    post = models.ForeignKey(Post, on_delete=models.CASCADE, related_name='comentarios')
    autor = models.ForeignKey(User, on_delete=models.CASCADE, related_name='comentarios')
    conteudo = models.TextField()
    aprovado = models.BooleanField(default=False)
    criado_em = models.DateTimeField(auto_now_add=True)
    
    class Meta:
        ordering = ['-criado_em']
    
    def __str__(self):
        return f'Comentário de {self.autor.username} em {self.post.titulo}'
```

### Forms

```python
# blog/forms.py
from django import forms
from django.contrib.auth.forms import UserCreationForm
from django.contrib.auth.models import User
from .models import Post, Comentario

class PostForm(forms.ModelForm):
    class Meta:
        model = Post
        fields = ['titulo', 'conteudo', 'resumo', 'categorias', 'imagem', 'status']
        widgets = {
            'conteudo': forms.Textarea(attrs={'rows': 10}),
            'resumo': forms.Textarea(attrs={'rows': 3}),
        }
    
    def clean_titulo(self):
        titulo = self.cleaned_data['titulo']
        if len(titulo) < 10:
            raise forms.ValidationError('O título deve ter pelo menos 10 caracteres')
        return titulo

class ComentarioForm(forms.ModelForm):
    class Meta:
        model = Comentario
        fields = ['conteudo']
        widgets = {
            'conteudo': forms.Textarea(attrs={'rows': 3, 'placeholder': 'Escreva seu comentário...'})
        }

class RegistroForm(UserCreationForm):
    email = forms.EmailField(required=True)
    
    class Meta:
        model = User
        fields = ['username', 'email', 'password1', 'password2']
    
    def save(self, commit=True):
        user = super().save(commit=False)
        user.email = self.cleaned_data['email']
        if commit:
            user.save()
        return user
```

### Views

```python
# blog/views.py
from django.shortcuts import render, get_object_or_404, redirect
from django.contrib.auth.decorators import login_required
from django.contrib import messages
from django.db.models import Count, Q
from django.core.paginator import Paginator
from django.utils import timezone
from .models import Post, Categoria, Comentario
from .forms import PostForm, ComentarioForm, RegistroForm

def lista_posts(request):
    """View principal que lista posts publicados"""
    
    # Query base
    posts = Post.objects.filter(status='publicado').select_related('autor')
    
    # Filtros
    categoria_slug = request.GET.get('categoria')
    busca = request.GET.get('q')
    
    if categoria_slug:
        posts = posts.filter(categorias__slug=categoria_slug)
    
    if busca:
        posts = posts.filter(
            Q(titulo__icontains=busca) | 
            Q(conteudo__icontains=busca)
        )
    
    # Ordenação
    ordenar = request.GET.get('ordenar', '-publicado_em')
    posts = posts.order_by(ordenar)
    
    # Paginação
    paginator = Paginator(posts, 10)
    page = request.GET.get('page')
    posts_paginados = paginator.get_page(page)
    
    # Dados para sidebar
    categorias = Categoria.objects.annotate(
        total_posts=Count('posts', filter=Q(posts__status='publicado'))
    ).filter(total_posts__gt=0)
    
    posts_recentes = Post.objects.filter(status='publicado').order_by('-publicado_em')[:5]
    
    context = {
        'posts': posts_paginados,
        'categorias': categorias,
        'posts_recentes': posts_recentes,
        'categoria_atual': categoria_slug,
        'busca': busca,
    }
    
    return render(request, 'blog/lista_posts.html', context)

def detalhe_post(request, slug):
    """View que mostra um post completo"""
    
    post = get_object_or_404(
        Post.objects.select_related('autor').prefetch_related('comentarios__autor'),
        slug=slug,
        status='publicado'
    )
    
    # Incrementar visualizações
    post.views += 1
    post.save(update_fields=['views'])
    
    # Processar comentário
    if request.method == 'POST' and request.user.is_authenticated:
        form = ComentarioForm(request.POST)
        if form.is_valid():
            comentario = form.save(commit=False)
            comentario.post = post
            comentario.autor = request.user
            comentario.save()
            messages.success(request, 'Comentário enviado para aprovação!')
            return redirect('blog:detalhe_post', slug=post.slug)
    else:
        form = ComentarioForm()
    
    # Posts relacionados (mesmas categorias)
    posts_relacionados = Post.objects.filter(
        categorias__in=post.categorias.all(),
        status='publicado'
    ).exclude(id=post.id).distinct()[:3]
    
    context = {
        'post': post,
        'comentarios': post.comentarios.filter(aprovado=True),
        'form': form,
        'posts_relacionados': posts_relacionados,
    }
    
    return render(request, 'blog/detalhe_post.html', context)

@login_required
def criar_post(request):
    """View para criar novo post"""
    
    if request.method == 'POST':
        form = PostForm(request.POST, request.FILES)
        if form.is_valid():
            post = form.save(commit=False)
            post.autor = request.user
            
            # Se status for publicado, definir data
            if post.status == 'publicado':
                post.publicado_em = timezone.now()
            
            post.save()
            form.save_m2m()  # Salvar many-to-many (categorias)
            
            messages.success(request, 'Post criado com sucesso!')
            return redirect('blog:detalhe_post', slug=post.slug)
    else:
        form = PostForm()
    
    return render(request, 'blog/form_post.html', {'form': form, 'acao': 'Criar'})

@login_required
def editar_post(request, slug):
    """View para editar post"""
    
    post = get_object_or_404(Post, slug=slug)
    
    # Verificar permissão
    if request.user != post.autor and not request.user.is_staff:
        messages.error(request, 'Você não tem permissão para editar este post.')
        return redirect('blog:detalhe_post', slug=post.slug)
    
    if request.method == 'POST':
        form = PostForm(request.POST, request.FILES, instance=post)
        if form.is_valid():
            post = form.save(commit=False)
            
            # Se estava rascunho e agora publicado
            if post.status == 'publicado' and not post.publicado_em:
                post.publicado_em = timezone.now()
            
            post.save()
            form.save_m2m()
            
            messages.success(request, 'Post atualizado com sucesso!')
            return redirect('blog:detalhe_post', slug=post.slug)
    else:
        form = PostForm(instance=post)
    
    return render(request, 'blog/form_post.html', {'form': form, 'acao': 'Editar', 'post': post})

def posts_por_categoria(request, slug):
    """View que lista posts de uma categoria específica"""
    categoria = get_object_or_404(Categoria, slug=slug)
    posts = Post.objects.filter(categorias=categoria, status='publicado')
    
    context = {
        'categoria': categoria,
        'posts': posts,
    }
    
    return render(request, 'blog/lista_categoria.html', context)

def registrar(request):
    """View de registro de usuário"""
    if request.method == 'POST':
        form = RegistroForm(request.POST)
        if form.is_valid():
            user = form.save()
            messages.success(request, 'Registro realizado com sucesso! Faça login para continuar.')
            return redirect('login')
    else:
        form = RegistroForm()
    
    return render(request, 'registration/registro.html', {'form': form})

@login_required
def meus_posts(request):
    """View que lista posts do usuário logado"""
    posts = Post.objects.filter(autor=request.user).order_by('-criado_em')
    
    context = {
        'posts': posts,
    }
    
    return render(request, 'blog/meus_posts.html', context)
```

### Templates

```html
<!-- templates/blog/base_blog.html -->
{% extends 'base.html' %}

{% block extra_css %}
<style>
    .post-card {
        transition: transform 0.3s;
    }
    .post-card:hover {
        transform: translateY(-5px);
        box-shadow: 0 4px 15px rgba(0,0,0,0.1);
    }
    .sidebar-widget {
        background: #f8f9fa;
        padding: 20px;
        border-radius: 5px;
        margin-bottom: 20px;
    }
    .badge-categoria {
        background: #e9ecef;
        color: #495057;
        padding: 5px 10px;
        border-radius: 3px;
        margin: 2px;
        display: inline-block;
    }
</style>
{% endblock %}
```

```html
<!-- templates/blog/lista_posts.html -->
{% extends 'blog/base_blog.html' %}

{% block title %}Blog - Meu Blog{% endblock %}

{% block content %}
<div class="row">
    <!-- Conteúdo principal -->
    <div class="col-md-8">
        <h1 class="mb-4">
            {% if categoria_atual %}
                Posts em: {{ categoria_atual }}
            {% elif busca %}
                Busca: "{{ busca }}"
            {% else %}
                Todos os Posts
            {% endif %}
        </h1>
        
        {% for post in posts %}
            <article class="card post-card mb-4">
                {% if post.imagem %}
                    <img src="{{ post.imagem.url }}" class="card-img-top" alt="{{ post.titulo }}">
                {% endif %}
                <div class="card-body">
                    <h2 class="card-title">{{ post.titulo }}</h2>
                    <p class="card-text text-muted">
                        <small>
                            Por {{ post.autor.get_full_name|default:post.autor.username }} | 
                            {{ post.publicado_em|date:"d/m/Y" }} | 
                            <i class="fas fa-eye"></i> {{ post.views }} visualizações |
                            <i class="fas fa-comments"></i> {{ post.comentarios.count }} comentários
                        </small>
                    </p>
                    <p class="card-text">{{ post.resumo|default:post.conteudo|truncatewords:50 }}</p>
                    
                    <div class="mb-3">
                        {% for categoria in post.categorias.all %}
                            <a href="?categoria={{ categoria.slug }}" class="badge-categoria text-decoration-none">
                                {{ categoria.nome }}
                            </a>
                        {% endfor %}
                    </div>
                    
                    <a href="{% url 'blog:detalhe_post' post.slug %}" class="btn btn-primary">
                        Leia mais →
                    </a>
                </div>
            </article>
        {% empty %}
            <div class="alert alert-info">
                Nenhum post encontrado.
            </div>
        {% endfor %}
        
        <!-- Paginação -->
        {% if posts.has_other_pages %}
            <nav aria-label="Paginação">
                <ul class="pagination justify-content-center">
                    {% if posts.has_previous %}
                        <li class="page-item">
                            <a class="page-link" href="?page=1{% if busca %}&q={{ busca }}{% endif %}{% if categoria_atual %}&categoria={{ categoria_atual }}{% endif %}">
                                &laquo;
                            </a>
                        </li>
                    {% endif %}
                    
                    {% for num in posts.paginator.page_range %}
                        {% if posts.number == num %}
                            <li class="page-item active">
                                <span class="page-link">{{ num }}</span>
                            </li>
                        {% elif num > posts.number|add:'-3' and num < posts.number|add:'3' %}
                            <li class="page-item">
                                <a class="page-link" href="?page={{ num }}{% if busca %}&q={{ busca }}{% endif %}{% if categoria_atual %}&categoria={{ categoria_atual }}{% endif %}">
                                    {{ num }}
                                </a>
                            </li>
                        {% endif %}
                    {% endfor %}
                    
                    {% if posts.has_next %}
                        <li class="page-item">
                            <a class="page-link" href="?page={{ posts.paginator.num_pages }}{% if busca %}&q={{ busca }}{% endif %}{% if categoria_atual %}&categoria={{ categoria_atual }}{% endif %}">
                                &raquo;
                            </a>
                        </li>
                    {% endif %}
                </ul>
            </nav>
        {% endif %}
    </div>
    
    <!-- Sidebar -->
    <div class="col-md-4">
        <!-- Busca -->
        <div class="sidebar-widget">
            <h5>Buscar</h5>
            <form method="get">
                <div class="input-group">
                    <input type="text" name="q" class="form-control" placeholder="Buscar posts..." value="{{ busca|default:'' }}">
                    <button class="btn btn-primary" type="submit">
                        <i class="fas fa-search"></i>
                    </button>
                </div>
            </form>
        </div>
        
        <!-- Categorias -->
        <div class="sidebar-widget">
            <h5>Categorias</h5>
            <div class="d-flex flex-wrap">
                {% for categoria in categorias %}
                    <a href="?categoria={{ categoria.slug }}" class="badge-categoria text-decoration-none">
                        {{ categoria.nome }} ({{ categoria.total_posts }})
                    </a>
                {% endfor %}
            </div>
        </div>
        
        <!-- Posts Recentes -->
        <div class="sidebar-widget">
            <h5>Posts Recentes</h5>
            {% for post in posts_recentes %}
                <div class="mb-3">
                    <a href="{% url 'blog:detalhe_post' post.slug %}" class="text-decoration-none">
                        <h6 class="mb-1">{{ post.titulo }}</h6>
                        <small class="text-muted">{{ post.publicado_em|date:"d/m/Y" }}</small>
                    </a>
                </div>
            {% endfor %}
        </div>
        
        <!-- Ações do usuário -->
        {% if user.is_authenticated %}
            <div class="sidebar-widget">
                <h5>Minha Conta</h5>
                <div class="list-group">
                    <a href="{% url 'blog:criar_post' %}" class="list-group-item list-group-item-action">
                        <i class="fas fa-plus"></i> Novo Post
                    </a>
                    <a href="{% url 'blog:meus_posts' %}" class="list-group-item list-group-item-action">
                        <i class="fas fa-edit"></i> Meus Posts
                    </a>
                </div>
            </div>
        {% endif %}
    </div>
</div>
{% endblock %}
```

```html
<!-- templates/blog/detalhe_post.html -->
{% extends 'blog/base_blog.html' %}

{% block title %}{{ post.titulo }} - Meu Blog{% endblock %}

{% block content %}
<div class="row">
    <div class="col-md-8">
        <!-- Post -->
        <article class="card mb-4">
            {% if post.imagem %}
                <img src="{{ post.imagem.url }}" class="card-img-top" alt="{{ post.titulo }}">
            {% endif %}
            <div class="card-body">
                <h1 class="card-title">{{ post.titulo }}</h1>
                
                <p class="text-muted">
                    <small>
                        Por {{ post.autor.get_full_name|default:post.autor.username }} | 
                        Publicado em {{ post.publicado_em|date:"d/m/Y H:i" }} | 
                        Última atualização: {{ post.atualizado_em|date:"d/m/Y H:i" }} |
                        <i class="fas fa-eye"></i> {{ post.views }} visualizações
                    </small>
                </p>
                
                <div class="mb-3">
                    {% for categoria in post.categorias.all %}
                        <a href="{% url 'blog:posts_por_categoria' categoria.slug %}" class="badge-categoria text-decoration-none">
                            {{ categoria.nome }}
                        </a>
                    {% endfor %}
                </div>
                
                <div class="card-text">
                    {{ post.conteudo|linebreaks }}
                </div>
                
                <!-- Ações do autor -->
                {% if user == post.autor or user.is_staff %}
                    <hr>
                    <div class="btn-group">
                        <a href="{% url 'blog:editar_post' post.slug %}" class="btn btn-warning">
                            <i class="fas fa-edit"></i> Editar
                        </a>
                    </div>
                {% endif %}
            </div>
        </article>
        
        <!-- Posts Relacionados -->
        {% if posts_relacionados %}
            <div class="card mb-4">
                <div class="card-header">
                    <h5>Posts Relacionados</h5>
                </div>
                <div class="card-body">
                    <div class="row">
                        {% for post_rel in posts_relacionados %}
                            <div class="col-md-4">
                                <div class="card h-100">
                                    {% if post_rel.imagem %}
                                        <img src="{{ post_rel.imagem.url }}" class="card-img-top" alt="{{ post_rel.titulo }}">
                                    {% endif %}
                                    <div class="card-body">
                                        <h6 class="card-title">{{ post_rel.titulo|truncatechars:30 }}</h6>
                                        <a href="{% url 'blog:detalhe_post' post_rel.slug %}" class="btn btn-sm btn-outline-primary">
                                            Ler
                                        </a>
                                    </div>
                                </div>
                            </div>
                        {% endfor %}
                    </div>
                </div>
            </div>
        {% endif %}
        
        <!-- Comentários -->
        <div class="card">
            <div class="card-header">
                <h5>Comentários ({{ comentarios.count }})</h5>
            </div>
            <div class="card-body">
                <!-- Lista de comentários -->
                {% for comentario in comentarios %}
                    <div class="mb-3 pb-3 border-bottom">
                        <p class="mb-1">
                            <strong>{{ comentario.autor.get_full_name|default:comentario.autor.username }}</strong>
                            <small class="text-muted"> - {{ comentario.criado_em|date:"d/m/Y H:i" }}</small>
                        </p>
                        <p class="mb-0">{{ comentario.conteudo }}</p>
                    </div>
                {% empty %}
                    <p class="text-muted">Nenhum comentário ainda. Seja o primeiro!</p>
                {% endfor %}
                
                <!-- Formulário de comentário -->
                {% if user.is_authenticated %}
                    <hr>
                    <h6>Deixe seu comentário</h6>
                    <form method="post">
                        {% csrf_token %}
                        {{ form.as_p }}
                        <button type="submit" class="btn btn-primary">Enviar comentário</button>
                    </form>
                {% else %}
                    <p class="alert alert-info">
                        <a href="{% url 'login' %}?next={{ request.path }}">Faça login</a> para comentar.
                    </p>
                {% endif %}
            </div>
        </div>
    </div>
    
    <!-- Sidebar (igual à lista) -->
    <div class="col-md-4">
        <!-- ... mesma sidebar da lista de posts ... -->
    </div>
</div>
{% endblock %}
```

### URLs

```python
# blog/urls.py
from django.urls import path
from django.contrib.auth import views as auth_views
from . import views

app_name = 'blog'

urlpatterns = [
    # Páginas principais
    path('', views.lista_posts, name='lista_posts'),
    path('post/<slug:slug>/', views.detalhe_post, name='detalhe_post'),
    path('categoria/<slug:slug>/', views.posts_por_categoria, name='posts_por_categoria'),
    
    # CRUD de posts
    path('post/novo/', views.criar_post, name='criar_post'),
    path('post/<slug:slug>/editar/', views.editar_post, name='editar_post'),
    path('meus-posts/', views.meus_posts, name='meus_posts'),
    
    # Autenticação
    path('registrar/', views.registrar, name='registrar'),
    path('login/', auth_views.LoginView.as_view(template_name='registration/login.html'), name='login'),
    path('logout/', auth_views.LogoutView.as_view(), name='logout'),
]
```

### Settings

```python
# settings.py
import os
from pathlib import Path

BASE_DIR = Path(__file__).resolve().parent.parent

# Segurança - em produção, use variáveis de ambiente
SECRET_KEY = 'sua-chave-secreta-aqui'
DEBUG = True
ALLOWED_HOSTS = []

# Apps
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    
    # Apps do projeto
    'blog',
]

# Middleware
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]

# URLs
ROOT_URLCONF = 'meuprojeto.urls'

# Templates
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [BASE_DIR / 'templates'],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]

# Banco de dados
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}

# Arquivos estáticos e mídia
STATIC_URL = '/static/'
STATICFILES_DIRS = [BASE_DIR / 'static']
STATIC_ROOT = BASE_DIR / 'staticfiles'

MEDIA_URL = '/media/'
MEDIA_ROOT = BASE_DIR / 'media'

# Autenticação
LOGIN_URL = 'login'
LOGIN_REDIRECT_URL = 'blog:lista_posts'
LOGOUT_REDIRECT_URL = 'blog:lista_posts'

# Mensagens
from django.contrib.messages import constants as messages
MESSAGE_TAGS = {
    messages.DEBUG: 'debug',
    messages.INFO: 'info',
    messages.SUCCESS: 'success',
    messages.WARNING: 'warning',
    messages.ERROR: 'danger',
}


**Referências úteis:**
- [Documentação oficial do Django](https://docs.djangoproject.com/)
- [Django REST Framework](https://www.django-rest-framework.org/)
- [Two Scoops of Django](https://www.feldroy.com/books/two-scoops-of-django-3-x) (livro recomendado)
