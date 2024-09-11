Vagrant.configure("2") do |config|
  # Configuración de la red privada para que las máquinas se puedan comunicar entre ellas
  config.vm.network "private_network", type: "dhcp"
  
  # Máquina virtual para el Auth API
  config.vm.define "auth-api" do |auth|
    auth.vm.box = "ubuntu/bionic64" # Puedes cambiar la versión de Ubuntu si lo prefieres
    auth.vm.hostname = "auth-api"
    auth.vm.network "private_network", ip: "192.168.50.2" # IP estática para esta máquina
    auth.vm.provision "docker"
    auth.vm.provision "shell", inline: <<-SHELL
      docker pull jorgeeduardo24/auth-image:0.1.0
      docker run -d -p 8000:8000 --name auth-api \
        -e AUTH_API_PORT=8000 \
        -e USERS_API_ADDRESS=http://192.168.50.3:8083 \
        -e JWT_SECRET=PRFT \
        jorgeeduardo24/auth-image:0.1.0
    SHELL
  end

  # Máquina virtual para el Users API
  config.vm.define "users-api" do |users|
    users.vm.box = "ubuntu/bionic64"
    users.vm.hostname = "users-api"
    users.vm.network "private_network", ip: "192.168.50.3"
    users.vm.provision "docker"
    users.vm.provision "shell", inline: <<-SHELL
      docker pull jorgeeduardo24/users-image:0.1.0
      docker run -d -p 8083:8083 --name users-api \
        -e JWT_SECRET=PRFT \
        -e SERVER_PORT=8083 \
        jorgeeduardo24/users-image:0.1.0
    SHELL
  end

  # Máquina virtual para el Log Message Processor
  config.vm.define "log-message-processor" do |log_processor|
    log_processor.vm.box = "ubuntu/bionic64"
    log_processor.vm.hostname = "log-message-processor"
    log_processor.vm.network "private_network", ip: "192.168.50.4"
    log_processor.vm.provision "docker"
    log_processor.vm.provision "shell", inline: <<-SHELL
      docker pull jorgeeduardo24/log-message-image:0.1.0
      docker run -d --name log-message-processor \
        -e REDIS_HOST=192.168.50.5 \
        -e REDIS_PORT=6379 \
        -e REDIS_CHANNEL=log_channel \
        jorgeeduardo24/log-message-image:0.1.0
    SHELL
  end

  # Máquina virtual para el Todos API
  config.vm.define "todos-api" do |todos|
    todos.vm.box = "ubuntu/bionic64"
    todos.vm.hostname = "todos-api"
    todos.vm.network "private_network", ip: "192.168.50.6"
    todos.vm.provision "docker"
    todos.vm.provision "shell", inline: <<-SHELL
      docker pull jorgeeduardo24/todos-image:0.1.0
      docker run -d -p 8082:8082 --name todos-api \
        -e TODO_API_PORT=8082 \
        -e JWT_SECRET=PRFT \
        -e REDIS_HOST=192.168.50.5 \
        -e REDIS_PORT=6379 \
        -e REDIS_CHANNEL=log_channel \
        jorgeeduardo24/todos-image:0.1.0
    SHELL
  end

  # Máquina virtual para el Frontend
  config.vm.define "frontend" do |frontend|
    frontend.vm.box = "ubuntu/bionic64"
    frontend.vm.hostname = "frontend"
    frontend.vm.network "private_network", ip: "192.168.50.7"
    frontend.vm.provision "docker"
    frontend.vm.provision "shell", inline: <<-SHELL
      docker pull jorgeeduardo24/frontend-image:0.1.0
      docker run -d -p 8080:8080 --name frontend \
        -e PORT=8080 \
        -e AUTH_API_ADDRESS=http://192.168.50.2:8000 \
        -e TODOS_API_ADDRESS=http://192.168.50.6:8082 \
        jorgeeduardo24/frontend-image:0.1.0
    SHELL
  end

  # Máquina virtual para Redis (Log Message Processor depende de Redis)
  config.vm.define "redis" do |redis|
    redis.vm.box = "ubuntu/bionic64"
    redis.vm.hostname = "redis"
    redis.vm.network "private_network", ip: "192.168.50.5"
    redis.vm.provision "docker"
    redis.vm.provision "shell", inline: <<-SHELL
      docker run -d --name redis -p 6379:6379 redis:7.0
    SHELL
  end
end
