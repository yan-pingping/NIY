# 飞机大战网页游戏 - 技术文档

## 1. 项目概述

### 1.1 项目背景
本项目是一款基于 HTML5 Canvas 的飞机大战网页小游戏，采用科技风格设计，融合市面上爆火的飞行射击游戏玩法，包含多种游戏模式、BOSS战系统和丰富的道具系统。

### 1.2 目标用户
- 休闲游戏玩家
- 飞行射击游戏爱好者
- 需要快速上手的轻度游戏用户

### 1.3 技术栈
| 分类 | 技术 | 版本 | 说明 |
|------|------|------|------|
| 前端框架 | HTML5 | - | 页面结构 |
| 游戏渲染 | Canvas 2D | - | 游戏画面绘制 |
| 样式 | CSS3 | - | 界面样式和动画 |
| 逻辑 | JavaScript (ES6+) | - | 游戏逻辑实现 |
| 图标 | SVG | - | UI图标和特效 |

### 1.4 项目结构
```
/
├── index.html          # 主页面入口
├── src/
│   ├── main.js         # 游戏主入口
│   ├── game/
│   │   ├── Game.js     # 游戏核心类
│   │   ├── Scene.js    # 场景管理
│   │   ├── Entity.js   # 实体基类
│   │   ├── Player.js   # 玩家飞机
│   │   ├── Enemy.js    # 敌机类
│   │   ├── Boss.js     # BOSS类
│   │   ├── Bullet.js   # 子弹类
│   │   ├── PowerUp.js  # 道具类
│   │   └── Particle.js # 粒子特效
│   ├── ui/
│   │   ├── HUD.js      # 游戏界面显示
│   │   ├── Menu.js     # 菜单系统
│   │   └── Dialog.js   # 对话框
│   ├── audio/
│   │   └── Audio.js    # 音频管理
│   ├── utils/
│   │   ├── Collision.js # 碰撞检测
│   │   ├── Vector.js    # 向量工具
│   │   └── Timer.js     # 定时器
│   └── config/
│       └── constants.js # 游戏常量配置
├── assets/
│   ├── images/         # 游戏图片资源
│   ├── sounds/         # 音效资源
│   └── fonts/          # 字体资源
└── styles/
    └── main.css        # 全局样式
```

---

## 2. 游戏玩法设计

### 2.1 核心玩法

#### 2.1.1 操作方式
- **键盘控制**:
  - `W/↑`: 向上移动
  - `S/↓`: 向下移动
  - `A/←`: 向左移动
  - `D/→`: 向右移动
  - `空格键`: 发射子弹
  - `Q`: 使用必杀技
- **鼠标控制**:
  - 鼠标移动: 控制飞机位置
  - 左键点击: 发射子弹
  - 右键点击: 使用必杀技
- **触屏控制**:
  - 滑动: 控制飞机位置
  - 点击: 发射子弹
  - 长按: 使用必杀技

#### 2.1.2 游戏目标
玩家控制飞机消灭敌机和BOSS，获取高分和道具，存活尽可能长的时间。

### 2.2 游戏模式

#### 2.2.1 经典模式 (Classic Mode)
- 无限生存模式
- 难度随时间递增
- 每1分钟出现一个小BOSS
- 每5分钟出现一个大BOSS

#### 2.2.2 闯关模式 (Level Mode)
- 预设关卡设计
- 每个关卡有特定目标
- 关卡包含普通敌机波次和BOSS战
- 通关条件：消灭所有敌机和BOSS

#### 2.2.3 无尽模式 (Endless Mode)
- 无限制波次
- 敌机类型随机生成
- BOSS出现间隔递减
- 记录最高分

### 2.3 玩家系统

#### 2.3.1 生命值系统
- 初始生命值: 3条命
- 可通过道具增加生命（最多5条）
- 被击中减少1条命
- 生命归零游戏结束

#### 2.3.2 武器系统
| 武器类型 | 伤害 | 射速 | 弹数 | 获取方式 |
|---------|------|------|------|---------|
| 普通机枪 | 1 | 快 | 1 | 默认 |
| 双发机枪 | 1.5 | 快 | 2 | 道具 |
| 三发散射 | 1 | 中 | 3 | 道具 |
| 激光炮 | 5 | 慢 | 1 | 道具 |
| 导弹 | 10 | 很慢 | 1 | 道具 |

#### 2.3.3 必杀技系统
- 能量条充能机制
- 击杀敌机获取能量
- 能量满时可释放必杀技
- 必杀技效果：全屏清屏+造成大量伤害

### 2.4 敌机系统

#### 2.4.1 敌机类型
| 类型 | 血量 | 速度 | 分数 | 特点 |
|------|------|------|------|------|
| 小型敌机 | 1 | 快 | 100 | 直线飞行 |
| 中型敌机 | 3 | 中 | 300 | 曲线飞行 |
| 大型敌机 | 6 | 慢 | 500 | 发射子弹 |
| 精英敌机 | 10 | 中 | 1000 | 多种攻击模式 |

#### 2.4.2 敌机行为模式
- 直线飞行: 从屏幕上方垂直下落
- 曲线飞行: 蛇形或波浪形轨迹
- 追踪飞行: 追踪玩家位置
- 编队飞行: 多架敌机编队进攻

### 2.5 道具系统

#### 2.5.1 道具类型
| 图标 | 类型 | 效果 | 持续时间 |
|------|------|------|---------|
| ❤️ | 生命 | +1生命值 | 永久 |
| ⚡ | 能量 | +50%能量 | 永久 |
| 🔫 | 武器升级 | 武器等级+1 | 30秒 |
| 🛡️ | 护盾 | 无敌3秒 | 3秒 |
| ⚔️ | 攻击力 | 攻击力翻倍 | 15秒 |
| 💰 | 金币 | +500分数 | 永久 |

#### 2.5.2 道具获取方式
- 击杀敌机随机掉落
- BOSS击杀必掉3个道具
- 特定位置刷新

---

## 3. BOSS战设计

### 3.1 BOSS类型

#### 3.1.1 小型BOSS (Mini Boss)
- 出现频率: 每60秒
- 血量: 50
- 攻击模式:
  - 散射弹幕
  - 追踪弹
- 奖励: 2000分 + 3个道具

#### 3.1.2 中型BOSS (Mid Boss)
- 出现频率: 每3分钟
- 血量: 150
- 攻击模式:
  - 扇形弹幕
  - 激光扫射
  - 召唤小怪
- 奖励: 5000分 + 5个道具

#### 3.1.3 大型BOSS (Final Boss)
- 出现频率: 每10分钟或通关关卡
- 血量: 500
- 攻击阶段:
  - 阶段1: 普通弹幕
  - 阶段2: 激光+弹幕组合
  - 阶段3: 全屏攻击+召唤精英怪
- 奖励: 15000分 + 稀有道具

### 3.2 BOSS攻击模式

#### 3.2.1 弹幕攻击
```
模式1: 圆形散射
      ●
    ● ● ●
  ● ● ● ● ●
    ● ● ●
      ●

模式2: 扇形扫射
        ●
      ● ● ●
    ● ● ● ● ●
  ● ● ● ● ● ● ●
```

#### 3.2.2 激光攻击
- 直线激光: 从BOSS发射直线激光
- 旋转激光: 激光围绕BOSS旋转
- 追踪激光: 跟随玩家移动

#### 3.2.3 召唤能力
- 召唤普通敌机
- 召唤精英敌机
- 召唤保护罩（需要先击破）

---

## 4. 游戏架构设计

### 4.1 核心类设计

#### 4.1.1 Game 类（游戏核心）
```javascript
class Game {
  constructor(canvas) {
    this.canvas = canvas;
    this.ctx = canvas.getContext('2d');
    this.width = canvas.width;
    this.height = canvas.height;
    this.running = false;
    this.paused = false;
    this.score = 0;
    this.level = 1;
    this.entities = [];      // 所有实体
    this.bullets = [];       // 子弹列表
    this.powerUps = [];      // 道具列表
    this.particles = [];     // 粒子列表
    this.scene = null;       // 当前场景
    this.player = null;      // 玩家对象
    this.lastTime = 0;
    this.deltaTime = 0;
  }
  
  // 游戏循环
  loop(currentTime) {
    this.deltaTime = (currentTime - this.lastTime) / 1000;
    this.lastTime = currentTime;
    
    this.update(this.deltaTime);
    this.render();
    
    if (this.running) {
      requestAnimationFrame(this.loop.bind(this));
    }
  }
  
  update(dt) {
    // 更新所有实体
    this.entities.forEach(entity => entity.update(dt));
    // 更新子弹
    this.bullets.forEach(bullet => bullet.update(dt));
    // 更新道具
    this.powerUps.forEach(powerUp => powerUp.update(dt));
    // 更新粒子
    this.particles.forEach(particle => particle.update(dt));
    
    // 碰撞检测
    this.checkCollisions();
    
    // 清理过期对象
    this.cleanup();
  }
  
  render() {
    // 清空画布
    this.ctx.fillStyle = '#0a0a1a';
    this.ctx.fillRect(0, 0, this.width, this.height);
    
    // 绘制背景
    this.drawBackground();
    
    // 绘制所有实体
    this.entities.forEach(entity => entity.render(this.ctx));
    // 绘制子弹
    this.bullets.forEach(bullet => bullet.render(this.ctx));
    // 绘制道具
    this.powerUps.forEach(powerUp => powerUp.render(this.ctx));
    // 绘制粒子
    this.particles.forEach(particle => particle.render(this.ctx));
    
    // 绘制UI
    this.hud.render(this.ctx);
  }
}
```

#### 4.1.2 Entity 类（实体基类）
```javascript
class Entity {
  constructor(x, y, width, height) {
    this.x = x;
    this.y = y;
    this.width = width;
    this.height = height;
    this.speed = 0;
    this.vx = 0;
    this.vy = 0;
    this.health = 1;
    this.maxHealth = 1;
    this.alive = true;
    this.sprite = null;
    this.rotation = 0;
  }
  
  update(dt) {
    this.x += this.vx * dt;
    this.y += this.vy * dt;
  }
  
  render(ctx) {
    ctx.save();
    ctx.translate(this.x, this.y);
    ctx.rotate(this.rotation);
    
    if (this.sprite) {
      ctx.drawImage(this.sprite, -this.width/2, -this.height/2, this.width, this.height);
    } else {
      // 默认绘制
      ctx.fillStyle = '#ff0000';
      ctx.fillRect(-this.width/2, -this.height/2, this.width, this.height);
    }
    
    ctx.restore();
  }
  
  takeDamage(amount) {
    this.health -= amount;
    if (this.health <= 0) {
      this.alive = false;
      this.onDeath();
    }
  }
  
  onDeath() {
    // 死亡回调，子类重写
  }
  
  getBounds() {
    return {
      x: this.x - this.width / 2,
      y: this.y - this.height / 2,
      width: this.width,
      height: this.height
    };
  }
}
```

#### 4.1.3 Player 类（玩家）
```javascript
class Player extends Entity {
  constructor(x, y) {
    super(x, y, 50, 50);
    this.maxHealth = 3;
    this.health = 3;
    this.weaponLevel = 1;
    this.energy = 0;
    this.maxEnergy = 100;
    this.shieldActive = false;
    this.shieldTime = 0;
    this.fireRate = 0.15;
    this.lastFireTime = 0;
    this.invincible = false;
    this.invincibleTime = 0;
  }
  
  update(dt) {
    super.update(dt);
    
    // 边界限制
    this.x = Math.max(this.width/2, Math.min(game.width - this.width/2, this.x));
    this.y = Math.max(this.height/2, Math.min(game.height - this.height/2, this.y));
    
    // 无敌时间更新
    if (this.invincible) {
      this.invincibleTime -= dt;
      if (this.invincibleTime <= 0) {
        this.invincible = false;
      }
    }
    
    // 护盾时间更新
    if (this.shieldActive) {
      this.shieldTime -= dt;
      if (this.shieldTime <= 0) {
        this.shieldActive = false;
      }
    }
    
    // 自动射击
    this.autoFire(dt);
  }
  
  autoFire(dt) {
    this.lastFireTime += dt;
    if (this.lastFireTime >= this.fireRate) {
      this.fire();
      this.lastFireTime = 0;
    }
  }
  
  fire() {
    const bulletSpeed = 500;
    
    switch(this.weaponLevel) {
      case 1: // 单发
        game.addBullet(new Bullet(this.x, this.y - this.height/2, 0, -bulletSpeed, 'player'));
        break;
      case 2: // 双发
        game.addBullet(new Bullet(this.x - 10, this.y - this.height/2, 0, -bulletSpeed, 'player'));
        game.addBullet(new Bullet(this.x + 10, this.y - this.height/2, 0, -bulletSpeed, 'player'));
        break;
      case 3: // 三发散射
        game.addBullet(new Bullet(this.x, this.y - this.height/2, 0, -bulletSpeed, 'player'));
        game.addBullet(new Bullet(this.x - 15, this.y - this.height/2, -50, -bulletSpeed, 'player'));
        game.addBullet(new Bullet(this.x + 15, this.y - this.height/2, 50, -bulletSpeed, 'player'));
        break;
      case 4: // 激光
        game.addBullet(new LaserBullet(this.x, this.y - this.height/2, 0, -bulletSpeed, 'player'));
        break;
      case 5: // 导弹
        game.addBullet(new MissileBullet(this.x, this.y - this.height/2, 0, -bulletSpeed, 'player'));
        break;
    }
    
    // 播放音效
    audio.play('shoot');
  }
  
  useUltimate() {
    if (this.energy >= this.maxEnergy) {
      // 释放必杀技
      game.triggerUltimate();
      this.energy = 0;
      audio.play('ultimate');
    }
  }
  
  addEnergy(amount) {
    this.energy = Math.min(this.maxEnergy, this.energy + amount);
  }
  
  takeDamage(amount) {
    if (this.invincible || this.shieldActive) return;
    
    super.takeDamage(amount);
    this.invincible = true;
    this.invincibleTime = 2; // 2秒无敌
    
    if (this.health <= 0) {
      game.gameOver();
    }
    
    audio.play('damage');
  }
  
  render(ctx) {
    super.render(ctx);
    
    // 绘制护盾
    if (this.shieldActive) {
      ctx.beginPath();
      ctx.arc(this.x, this.y, this.width/2 + 10, 0, Math.PI * 2);
      ctx.strokeStyle = 'rgba(0, 255, 255, 0.5)';
      ctx.lineWidth = 3;
      ctx.stroke();
    }
    
    // 无敌闪烁效果
    if (this.invincible && Math.floor(Date.now() / 100) % 2 === 0) {
      ctx.globalAlpha = 0.5;
    }
    
    // 绘制能量条
    this.drawEnergyBar(ctx);
  }
  
  drawEnergyBar(ctx) {
    const barWidth = 60;
    const barHeight = 6;
    const barX = this.x - barWidth / 2;
    const barY = this.y + this.height / 2 + 10;
    
    ctx.fillStyle = 'rgba(0, 0, 0, 0.5)';
    ctx.fillRect(barX, barY, barWidth, barHeight);
    
    ctx.fillStyle = '#00ffff';
    ctx.fillRect(barX, barY, barWidth * (this.energy / this.maxEnergy), barHeight);
  }
}
```

#### 4.1.4 Enemy 类（敌机）
```javascript
class Enemy extends Entity {
  constructor(x, y, type) {
    const configs = {
      small: { width: 30, height: 30, health: 1, speed: 150, score: 100 },
      medium: { width: 45, height: 45, health: 3, speed: 100, score: 300 },
      large: { width: 60, height: 60, health: 6, speed: 70, score: 500 },
      elite: { width: 50, height: 50, health: 10, speed: 120, score: 1000 }
    };
    
    const config = configs[type];
    super(x, y, config.width, config.height);
    this.type = type;
    this.health = config.health;
    this.maxHealth = config.health;
    this.speed = config.speed;
    this.scoreValue = config.score;
    this.fireRate = type === 'large' || type === 'elite' ? 2 : 0;
    this.lastFireTime = 0;
    this.movementPattern = this.getRandomPattern();
    this.patternTimer = 0;
  }
  
  getRandomPattern() {
    const patterns = ['straight', 'zigzag', 'wave', 'track'];
    return patterns[Math.floor(Math.random() * patterns.length)];
  }
  
  update(dt) {
    // 移动模式
    switch(this.movementPattern) {
      case 'straight':
        this.vy = this.speed;
        break;
      case 'zigzag':
        this.vy = this.speed * 0.8;
        this.patternTimer += dt;
        this.vx = Math.sin(this.patternTimer * 3) * 50;
        break;
      case 'wave':
        this.vy = this.speed * 0.6;
        this.patternTimer += dt;
        this.vx = Math.sin(this.patternTimer * 2) * 80;
        break;
      case 'track':
        this.vy = this.speed * 0.5;
        const dx = game.player.x - this.x;
        this.vx = dx * 0.5;
        break;
    }
    
    super.update(dt);
    
    // 边界限制
    this.x = Math.max(this.width/2, Math.min(game.width - this.width/2, this.x));
    
    // 射击
    if (this.fireRate > 0) {
      this.lastFireTime += dt;
      if (this.lastFireTime >= this.fireRate) {
        this.fire();
        this.lastFireTime = 0;
      }
    }
  }
  
  fire() {
    game.addBullet(new Bullet(this.x, this.y + this.height/2, 0, 200, 'enemy'));
    audio.play('enemyShoot');
  }
  
  onDeath() {
    // 加分
    game.addScore(this.scoreValue);
    
    // 增加能量
    game.player.addEnergy(10);
    
    // 掉落道具（20%概率）
    if (Math.random() < 0.2) {
      game.addPowerUp(new PowerUp(this.x, this.y));
    }
    
    // 爆炸特效
    game.addExplosion(this.x, this.y, this.type === 'large' ? 'big' : 'small');
    
    audio.play('explosion');
  }
  
  render(ctx) {
    super.render(ctx);
    
    // 绘制血条
    if (this.health < this.maxHealth) {
      const barWidth = this.width;
      const barHeight = 4;
      const barX = this.x - barWidth / 2;
      const barY = this.y - this.height / 2 - 10;
      
      ctx.fillStyle = 'rgba(0, 0, 0, 0.5)';
      ctx.fillRect(barX, barY, barWidth, barHeight);
      
      ctx.fillStyle = '#ff4444';
      ctx.fillRect(barX, barY, barWidth * (this.health / this.maxHealth), barHeight);
    }
  }
}
```

#### 4.1.5 Boss 类（BOSS）
```javascript
class Boss extends Entity {
  constructor(x, y, type) {
    const configs = {
      mini: { width: 80, height: 80, health: 50, speed: 30, score: 2000, phases: 1 },
      mid: { width: 120, height: 100, health: 150, speed: 20, score: 5000, phases: 2 },
      final: { width: 150, height: 120, health: 500, speed: 15, score: 15000, phases: 3 }
    };
    
    const config = configs[type];
    super(x, y, config.width, config.height);
    this.type = type;
    this.health = config.health;
    this.maxHealth = config.health;
    this.speed = config.speed;
    this.scoreValue = config.score;
    this.phases = config.phases;
    this.currentPhase = 1;
    this.attackPattern = 'spread';
    this.attackTimer = 0;
    this.moveDirection = 1;
    this.isEntering = true;
    this.entryTime = 0;
  }
  
  update(dt) {
    // 入场动画
    if (this.isEntering) {
      this.entryTime += dt;
      if (this.entryTime < 2) {
        this.y += 50 * dt;
        return;
      }
      this.isEntering = false;
    }
    
    // 左右移动
    this.x += this.speed * this.moveDirection * dt;
    if (this.x > game.width - this.width/2 || this.x < this.width/2) {
      this.moveDirection *= -1;
    }
    
    super.update(dt);
    
    // 攻击逻辑
    this.attackTimer += dt;
    this.performAttack();
  }
  
  performAttack() {
    const attackInterval = 1.5 - this.currentPhase * 0.3;
    
    if (this.attackTimer >= attackInterval) {
      this.attackTimer = 0;
      
      // 根据阶段切换攻击模式
      const patterns = ['spread', 'fan', 'laser', 'summon'];
      const availablePatterns = patterns.slice(0, this.currentPhase + 1);
      this.attackPattern = availablePatterns[Math.floor(Math.random() * availablePatterns.length)];
      
      switch(this.attackPattern) {
        case 'spread':
          this.spreadAttack();
          break;
        case 'fan':
          this.fanAttack();
          break;
        case 'laser':
          this.laserAttack();
          break;
        case 'summon':
          this.summonAttack();
          break;
      }
      
      audio.play('bossAttack');
    }
  }
  
  spreadAttack() {
    const bulletCount = 8 + this.currentPhase * 4;
    for (let i = 0; i < bulletCount; i++) {
      const angle = (Math.PI * 2 / bulletCount) * i;
      const speed = 200;
      game.addBullet(new Bullet(
        this.x, this.y + this.height/2,
        Math.cos(angle) * speed,
        Math.sin(angle) * speed + 100,
        'enemy'
      ));
    }
  }
  
  fanAttack() {
    const bulletCount = 5;
    const baseAngle = Math.PI / 2;
    const spreadAngle = 0.5;
    for (let i = 0; i < bulletCount; i++) {
      const angle = baseAngle - spreadAngle + (spreadAngle * 2 / (bulletCount - 1)) * i;
      const speed = 250;
      game.addBullet(new Bullet(
        this.x, this.y + this.height/2,
        Math.cos(angle) * speed,
        Math.sin(angle) * speed,
        'enemy'
      ));
    }
  }
  
  laserAttack() {
    // 创建激光（持续一段时间）
    game.addLaser(this.x, this.y + this.height/2, game.player.x, game.height);
  }
  
  summonAttack() {
    // 召唤小怪
    for (let i = 0; i < 3; i++) {
      const x = Math.random() * (game.width - 60) + 30;
      game.addEnemy(new Enemy(x, -50, 'small'));
    }
  }
  
  takeDamage(amount) {
    super.takeDamage(amount);
    
    // 阶段切换
    const healthThresholds = [1, 0.6, 0.3];
    for (let i = 0; i < healthThresholds.length; i++) {
      if (this.health / this.maxHealth <= healthThresholds[i] && this.currentPhase < i + 1) {
        this.currentPhase = i + 1;
        audio.play('bossPhase');
        break;
      }
    }
  }
  
  onDeath() {
    game.addScore(this.scoreValue);
    game.player.addEnergy(50);
    
    // 必掉3个道具
    for (let i = 0; i < 3; i++) {
      setTimeout(() => {
        game.addPowerUp(new PowerUp(this.x + (Math.random() - 0.5) * 100, this.y + Math.random() * 50));
      }, i * 200);
    }
    
    // 大型爆炸特效
    game.addExplosion(this.x, this.y, 'massive');
    audio.play('bossExplosion');
    
    // BOSS战结束
    game.onBossDefeated();
  }
  
  render(ctx) {
    super.render(ctx);
    
    // 绘制血条
    const barWidth = 200;
    const barHeight = 15;
    const barX = game.width / 2 - barWidth / 2;
    const barY = 20;
    
    ctx.fillStyle = 'rgba(0, 0, 0, 0.7)';
    ctx.fillRect(barX, barY, barWidth, barHeight);
    
    ctx.fillStyle = '#ff0000';
    ctx.fillRect(barX, barY, barWidth * (this.health / this.maxHealth), barHeight);
    
    // 血条边框
    ctx.strokeStyle = '#ffffff';
    ctx.lineWidth = 2;
    ctx.strokeRect(barX, barY, barWidth, barHeight);
    
    // BOSS名称
    ctx.fillStyle = '#ffffff';
    ctx.font = 'bold 16px Arial';
    ctx.textAlign = 'center';
    ctx.fillText(`BOSS - ${this.type.toUpperCase()}`, game.width / 2, barY - 8);
    
    // 阶段指示
    ctx.fillText(`Phase ${this.currentPhase}/${this.phases}`, game.width / 2, barY + barHeight + 16);
  }
}
```

#### 4.1.6 Bullet 类（子弹）
```javascript
class Bullet extends Entity {
  constructor(x, y, vx, vy, owner) {
    super(x, y, 8, 15);
    this.vx = vx;
    this.vy = vy;
    this.owner = owner; // 'player' or 'enemy'
    this.damage = owner === 'player' ? 1 : 1;
    this.color = owner === 'player' ? '#00ffff' : '#ff4444';
  }
  
  update(dt) {
    super.update(dt);
    
    // 移除出界子弹
    if (this.y < -50 || this.y > game.height + 50 ||
        this.x < -50 || this.x > game.width + 50) {
      this.alive = false;
    }
  }
  
  render(ctx) {
    ctx.save();
    ctx.translate(this.x, this.y);
    ctx.rotate(Math.atan2(this.vy, this.vx));
    
    // 绘制子弹
    const gradient = ctx.createLinearGradient(-this.width/2, -this.height/2, this.width/2, this.height/2);
    gradient.addColorStop(0, this.color);
    gradient.addColorStop(1, 'rgba(255, 255, 255, 0.3)');
    
    ctx.fillStyle = gradient;
    ctx.beginPath();
    ctx.ellipse(0, 0, this.width/2, this.height/2, 0, 0, Math.PI * 2);
    ctx.fill();
    
    // 发光效果
    ctx.shadowColor = this.color;
    ctx.shadowBlur = 10;
    ctx.fill();
    
    ctx.restore();
  }
}

// 激光子弹
class LaserBullet extends Bullet {
  constructor(x, y, vx, vy, owner) {
    super(x, y, vx, vy, owner);
    this.width = 15;
    this.height = 40;
    this.damage = 5;
    this.color = '#00ff00';
  }
  
  render(ctx) {
    ctx.save();
    ctx.translate(this.x, this.y);
    ctx.rotate(Math.atan2(this.vy, this.vx));
    
    const gradient = ctx.createLinearGradient(0, -this.height/2, 0, this.height/2);
    gradient.addColorStop(0, '#ffffff');
    gradient.addColorStop(0.5, this.color);
    gradient.addColorStop(1, 'rgba(0, 255, 0, 0)');
    
    ctx.fillStyle = gradient;
    ctx.fillRect(-this.width/2, -this.height/2, this.width, this.height);
    
    ctx.shadowColor = this.color;
    ctx.shadowBlur = 20;
    ctx.fill();
    
    ctx.restore();
  }
}

// 导弹子弹
class MissileBullet extends Bullet {
  constructor(x, y, vx, vy, owner) {
    super(x, y, vx, vy, owner);
    this.width = 20;
    this.height = 35;
    this.damage = 10;
    this.color = '#ffff00';
    this.target = null;
    this.homingStrength = 0.8;
  }
  
  update(dt) {
    // 追踪目标
    if (!this.target && game.enemies.length > 0) {
      this.target = game.enemies[Math.floor(Math.random() * game.enemies.length)];
    }
    
    if (this.target && this.target.alive) {
      const dx = this.target.x - this.x;
      const dy = this.target.y - this.y;
      const angle = Math.atan2(dy, dx);
      
      this.vx += Math.cos(angle) * this.homingStrength * dt * 1000;
      this.vy += Math.sin(angle) * this.homingStrength * dt * 1000;
      
      // 限制速度
      const speed = Math.sqrt(this.vx * this.vx + this.vy * this.vy);
      if (speed > 300) {
        this.vx = (this.vx / speed) * 300;
        this.vy = (this.vy / speed) * 300;
      }
    }
    
    super.update(dt);
  }
  
  render(ctx) {
    ctx.save();
    ctx.translate(this.x, this.y);
    ctx.rotate(Math.atan2(this.vy, this.vx));
    
    // 导弹主体
    ctx.fillStyle = this.color;
    ctx.beginPath();
    ctx.moveTo(0, -this.height/2);
    ctx.lineTo(-this.width/2, this.height/2);
    ctx.lineTo(this.width/2, this.height/2);
    ctx.closePath();
    ctx.fill();
    
    // 尾焰
    const flameGradient = ctx.createLinearGradient(0, this.height/2, 0, this.height/2 + 20);
    flameGradient.addColorStop(0, '#ff8800');
    flameGradient.addColorStop(1, 'rgba(255, 0, 0, 0)');
    
    ctx.fillStyle = flameGradient;
    ctx.beginPath();
    ctx.moveTo(-5, this.height/2);
    ctx.lineTo(0, this.height/2 + 20 + Math.random() * 10);
    ctx.lineTo(5, this.height/2);
    ctx.closePath();
    ctx.fill();
    
    ctx.restore();
  }
}
```

#### 4.1.7 PowerUp 类（道具）
```javascript
class PowerUp extends Entity {
  constructor(x, y) {
    super(x, y, 25, 25);
    this.types = ['health', 'energy', 'weapon', 'shield', 'attack', 'coin'];
    this.type = this.types[Math.floor(Math.random() * this.types.length)];
    this.speed = 50;
    this.vy = this.speed;
    this.rotationSpeed = 2;
    this.floatOffset = 0;
  }
  
  update(dt) {
    super.update(dt);
    this.rotation += this.rotationSpeed * dt;
    this.floatOffset = Math.sin(Date.now() / 200) * 3;
    
    // 出界移除
    if (this.y > game.height + 50) {
      this.alive = false;
    }
  }
  
  applyEffect(player) {
    switch(this.type) {
      case 'health':
        player.health = Math.min(player.maxHealth, player.health + 1);
        break;
      case 'energy':
        player.addEnergy(player.maxEnergy * 0.5);
        break;
      case 'weapon':
        player.weaponLevel = Math.min(5, player.weaponLevel + 1);
        // 设置武器升级持续时间
        player.weaponUpgradeTime = 30;
        break;
      case 'shield':
        player.shieldActive = true;
        player.shieldTime = 3;
        break;
      case 'attack':
        player.damageMultiplier = 2;
        player.attackBoostTime = 15;
        break;
      case 'coin':
        game.addScore(500);
        break;
    }
    
    audio.play('powerUp');
    this.alive = false;
  }
  
  render(ctx) {
    ctx.save();
    ctx.translate(this.x, this.y + this.floatOffset);
    ctx.rotate(this.rotation);
    
    // 发光背景
    const gradient = ctx.createRadialGradient(0, 0, 0, 0, 0, this.width);
    gradient.addColorStop(0, this.getColor());
    gradient.addColorStop(1, 'rgba(0, 0, 0, 0)');
    ctx.fillStyle = gradient;
    ctx.beginPath();
    ctx.arc(0, 0, this.width, 0, Math.PI * 2);
    ctx.fill();
    
    // 图标
    ctx.fillStyle = '#ffffff';
    ctx.font = 'bold 16px Arial';
    ctx.textAlign = 'center';
    ctx.textBaseline = 'middle';
    ctx.fillText(this.getIcon(), 0, 0);
    
    ctx.restore();
  }
  
  getColor() {
    const colors = {
      health: '#ff4444',
      energy: '#ffff00',
      weapon: '#00ffff',
      shield: '#44ff44',
      attack: '#ff8800',
      coin: '#ffd700'
    };
    return colors[this.type];
  }
  
  getIcon() {
    const icons = {
      health: '❤️',
      energy: '⚡',
      weapon: '🔫',
      shield: '🛡️',
      attack: '⚔️',
      coin: '💰'
    };
    return icons[this.type];
  }
}
```

#### 4.1.8 Particle 类（粒子特效）
```javascript
class Particle extends Entity {
  constructor(x, y, color, size, speed, life) {
    super(x, y, size, size);
    this.color = color;
    this.speed = speed;
    this.life = life;
    this.maxLife = life;
    this.angle = Math.random() * Math.PI * 2;
    this.vx = Math.cos(this.angle) * speed;
    this.vy = Math.sin(this.angle) * speed;
    this.gravity = 0;
    this.friction = 0.98;
  }
  
  update(dt) {
    this.vx *= this.friction;
    this.vy *= this.friction;
    this.vy += this.gravity * dt;
    
    super.update(dt);
    
    this.life -= dt;
    if (this.life <= 0) {
      this.alive = false;
    }
  }
  
  render(ctx) {
    ctx.save();
    ctx.globalAlpha = this.life / this.maxLife;
    ctx.fillStyle = this.color;
    ctx.beginPath();
    ctx.arc(this.x, this.y, this.width / 2, 0, Math.PI * 2);
    ctx.fill();
    ctx.restore();
  }
}
```

### 4.2 碰撞检测系统

```javascript
class Collision {
  static rectRect(rect1, rect2) {
    return rect1.x < rect2.x + rect2.width &&
           rect1.x + rect1.width > rect2.x &&
           rect1.y < rect2.y + rect2.height &&
           rect1.y + rect1.height > rect2.y;
  }
  
  static circleCircle(circle1, circle2) {
    const dx = circle1.x - circle2.x;
    const dy = circle1.y - circle2.y;
    const distance = Math.sqrt(dx * dx + dy * dy);
    return distance < circle1.radius + circle2.radius;
  }
  
  static rectCircle(rect, circle) {
    const closestX = Math.max(rect.x, Math.min(circle.x, rect.x + rect.width));
    const closestY = Math.max(rect.y, Math.min(circle.y, rect.y + rect.height));
    
    const dx = circle.x - closestX;
    const dy = circle.y - closestY;
    const distance = Math.sqrt(dx * dx + dy * dy);
    
    return distance < circle.radius;
  }
}
```

### 4.3 场景管理

```javascript
class SceneManager {
  constructor() {
    this.scenes = {};
    this.currentScene = null;
  }
  
  addScene(name, scene) {
    this.scenes[name] = scene;
  }
  
  switchScene(name) {
    if (this.scenes[name]) {
      if (this.currentScene) {
        this.currentScene.onExit();
      }
      this.currentScene = this.scenes[name];
      this.currentScene.onEnter();
    }
  }
  
  update(dt) {
    if (this.currentScene) {
      this.currentScene.update(dt);
    }
  }
  
  render(ctx) {
    if (this.currentScene) {
      this.currentScene.render(ctx);
    }
  }
}

// 场景基类
class Scene {
  constructor() {}
  
  onEnter() {}
  
  onExit() {}
  
  update(dt) {}
  
  render(ctx) {}
}

// 菜单场景
class MenuScene extends Scene {
  constructor() {
    super();
    this.menuItems = [
      { text: '经典模式', action: () => game.startGame('classic') },
      { text: '闯关模式', action: () => game.startGame('level') },
      { text: '无尽模式', action: () => game.startGame('endless') },
      { text: '排行榜', action: () => game.showLeaderboard() },
      { text: '设置', action: () => game.showSettings() }
    ];
    this.selectedIndex = 0;
  }
  
  onEnter() {
    this.selectedIndex = 0;
  }
  
  handleInput(key) {
    switch(key) {
      case 'ArrowUp':
      case 'w':
        this.selectedIndex = Math.max(0, this.selectedIndex - 1);
        break;
      case 'ArrowDown':
      case 's':
        this.selectedIndex = Math.min(this.menuItems.length - 1, this.selectedIndex + 1);
        break;
      case 'Enter':
      case ' ':
        this.menuItems[this.selectedIndex].action();
        break;
    }
  }
  
  render(ctx) {
    // 背景
    ctx.fillStyle = '#0a0a1a';
    ctx.fillRect(0, 0, game.width, game.height);
    
    // 星星背景
    this.drawStars(ctx);
    
    // 标题
    ctx.fillStyle = '#00ffff';
    ctx.font = 'bold 48px Arial';
    ctx.textAlign = 'center';
    ctx.shadowColor = '#00ffff';
    ctx.shadowBlur = 20;
    ctx.fillText('星际战机', game.width / 2, 100);
    
    ctx.font = '24px Arial';
    ctx.fillStyle = '#ffffff';
    ctx.shadowBlur = 0;
    ctx.fillText('STAR FIGHTER', game.width / 2, 140);
    
    // 菜单选项
    const menuY = 250;
    const menuSpacing = 50;
    
    this.menuItems.forEach((item, index) => {
      ctx.font = index === this.selectedIndex ? 'bold 32px Arial' : '28px Arial';
      ctx.fillStyle = index === this.selectedIndex ? '#00ffff' : '#aaaaaa';
      
      if (index === this.selectedIndex) {
        ctx.shadowColor = '#00ffff';
        ctx.shadowBlur = 10;
      } else {
        ctx.shadowBlur = 0;
      }
      
      ctx.fillText(item.text, game.width / 2, menuY + index * menuSpacing);
    });
    
    // 操作提示
    ctx.font = '18px Arial';
    ctx.fillStyle = '#666666';
    ctx.shadowBlur = 0;
    ctx.fillText('↑↓ 选择  Enter 确认', game.width / 2, game.height - 50);
  }
  
  drawStars(ctx) {
    // 绘制星星背景（简化版）
    for (let i = 0; i < 100; i++) {
      const x = (i * 137.5) % game.width;
      const y = (i * 97.3) % game.height;
      const size = (i % 3) + 1;
      ctx.fillStyle = `rgba(255, 255, 255, ${0.3 + (i % 5) * 0.15})`;
      ctx.beginPath();
      ctx.arc(x, y, size, 0, Math.PI * 2);
      ctx.fill();
    }
  }
}

// 游戏场景
class GameScene extends Scene {
  constructor() {
    super();
    this.spawnTimer = 0;
    this.bossTimer = 0;
    this.difficulty = 1;
    this.waveNumber = 1;
  }
  
  onEnter() {
    game.player = new Player(game.width / 2, game.height - 100);
    game.entities = [game.player];
    game.bullets = [];
    game.powerUps = [];
    game.particles = [];
    game.score = 0;
    game.level = 1;
    
    this.spawnTimer = 0;
    this.bossTimer = 0;
    this.difficulty = 1;
    this.waveNumber = 1;
    
    audio.play('gameStart');
  }
  
  update(dt) {
    // 敌人生成
    this.spawnTimer += dt;
    const spawnInterval = Math.max(0.3, 2 - this.difficulty * 0.2);
    
    if (this.spawnTimer >= spawnInterval) {
      this.spawnTimer = 0;
      this.spawnEnemy();
    }
    
    // BOSS生成
    this.bossTimer += dt;
    const bossInterval = Math.max(30, 120 - this.difficulty * 10);
    
    if (this.bossTimer >= bossInterval) {
      this.bossTimer = 0;
      this.spawnBoss();
    }
    
    // 难度递增
    this.difficulty += dt * 0.01;
    
    // 更新波次
    if (this.spawnTimer % 30 === 0) {
      this.waveNumber++;
      game.level = this.waveNumber;
    }
  }
  
  spawnEnemy() {
    const types = ['small', 'small', 'small', 'medium', 'medium', 'large', 'elite'];
    const weights = [0.5, 0.5, 0.5, 0.3, 0.3, 0.15, 0.05];
    
    // 根据难度调整权重
    const adjustedWeights = weights.map((w, i) => w * (1 + this.difficulty * 0.1 * i));
    
    // 随机选择类型
    const random = Math.random();
    let cumulative = 0;
    let selectedType = 'small';
    
    for (let i = 0; i < types.length; i++) {
      cumulative += adjustedWeights[i];
      if (random < cumulative) {
        selectedType = types[i];
        break;
      }
    }
    
    const x = Math.random() * (game.width - 60) + 30;
    game.addEnemy(new Enemy(x, -50, selectedType));
  }
  
  spawnBoss() {
    // 根据难度选择BOSS类型
    let bossType = 'mini';
    if (this.difficulty > 5) bossType = 'mid';
    if (this.difficulty > 10) bossType = 'final';
    
    game.addBoss(new Boss(game.width / 2, -150, bossType));
  }
  
  render(ctx) {
    // 游戏渲染由Game类负责
  }
}
```

---

## 5. UI界面设计

### 5.1 主菜单界面

```
┌─────────────────────────────────────────────┐
│                                             │
│              ⭐ 星际战机 ⭐                 │
│             STAR FIGHTER                   │
│                                             │
│           ┌─────────────────┐              │
│           │    经典模式     │              │
│           ├─────────────────┤              │
│           │    闯关模式     │              │
│           ├─────────────────┤              │
│           │    无尽模式     │              │
│           ├─────────────────┤              │
│           │    排行榜       │              │
│           ├─────────────────┤              │
│           │    设置         │              │
│           └─────────────────┘              │
│                                             │
│           ↑↓ 选择  Enter 确认              │
│                                             │
└─────────────────────────────────────────────┘
```

### 5.2 游戏HUD界面

```
┌─────────────────────────────────────────────┐
│  ❤️ x3  |  ⚡ [██████████]  |  分数: 12000  │
│                                             │
│                                             │
│                    游戏画面                  │
│                                             │
│                                             │
│  波次: 5  |  武器: Lv.3  |  BOSS: 20%      │
└─────────────────────────────────────────────┘
```

### 5.3 科技风格UI规范

#### 5.3.1 颜色方案
| 颜色 | 用途 |
|------|------|
| `#0a0a1a` | 背景色 |
| `#00ffff` | 主色调（玩家、UI高亮） |
| `#ff4444` | 敌人色调 |
| `#44ff44` | 绿色（护盾、治愈） |
| `#ffff00` | 黄色（能量、金币） |
| `#ffffff` | 白色（文本） |

#### 5.3.2 字体规范
- 标题: `bold 48px Arial`
- 菜单选项: `28-32px Arial`
- HUD文本: `18-20px Arial`
- 分数/数值: `bold 24px Arial`

#### 5.3.3 动画效果
- 发光效果: `shadowBlur: 10-20`
- 闪烁效果: `globalAlpha: 0.3-1.0`
- 渐变效果: `createLinearGradient/createRadialGradient`
- 粒子效果: 爆炸、尾焰、星星

---

## 6. 音频系统

```javascript
class AudioManager {
  constructor() {
    this.sounds = {};
    this.muted = false;
    this.volume = 0.5;
  }
  
  loadSound(name, src) {
    const audio = new Audio(src);
    audio.volume = this.volume;
    this.sounds[name] = audio;
  }
  
  play(name) {
    if (this.muted || !this.sounds[name]) return;
    
    const sound = this.sounds[name].cloneNode();
    sound.volume = this.volume;
    sound.play().catch(() => {});
  }
  
  setVolume(value) {
    this.volume = Math.max(0, Math.min(1, value));
    Object.values(this.sounds).forEach(sound => {
      sound.volume = this.volume;
    });
  }
  
  toggleMute() {
    this.muted = !this.muted;
  }
}

// 音效列表
const audioList = {
  shoot: 'assets/sounds/shoot.wav',
  enemyShoot: 'assets/sounds/enemy_shoot.wav',
  explosion: 'assets/sounds/explosion.wav',
  bossExplosion: 'assets/sounds/boss_explosion.wav',
  powerUp: 'assets/sounds/powerup.wav',
  damage: 'assets/sounds/damage.wav',
  bossPhase: 'assets/sounds/boss_phase.wav',
  bossAttack: 'assets/sounds/boss_attack.wav',
  gameStart: 'assets/sounds/game_start.wav',
  gameOver: 'assets/sounds/game_over.wav',
  ultimate: 'assets/sounds/ultimate.wav'
};
```

---

## 7. 配置与常量

```javascript
// 游戏配置常量
const GAME_CONFIG = {
  // 画布尺寸
  WIDTH: 800,
  HEIGHT: 600,
  
  // 玩家配置
  PLAYER: {
    INITIAL_HEALTH: 3,
    MAX_HEALTH: 5,
    MAX_ENERGY: 100,
    INVINCIBLE_TIME: 2,
    SHIELD_TIME: 3,
    WEAPON_UPGRADE_TIME: 30,
    ATTACK_BOOST_TIME: 15
  },
  
  // 子弹配置
  BULLET: {
    PLAYER_SPEED: 500,
    ENEMY_SPEED: 200,
    MAX_COUNT: 200
  },
  
  // 敌人生成配置
  SPAWN: {
    INITIAL_INTERVAL: 2,
    MIN_INTERVAL: 0.3,
    DIFFICULTY_INCREMENT: 0.01,
    BOSS_INTERVAL: 120,
    MIN_BOSS_INTERVAL: 30
  },
  
  // 分数配置
  SCORE: {
    SMALL_ENEMY: 100,
    MEDIUM_ENEMY: 300,
    LARGE_ENEMY: 500,
    ELITE_ENEMY: 1000,
    MINI_BOSS: 2000,
    MID_BOSS: 5000,
    FINAL_BOSS: 15000,
    COIN_POWERUP: 500
  },
  
  // 能量配置
  ENERGY: {
    PER_KILL: 10,
    BOSS_KILL: 50,
    ENERGY_POWERUP: 50
  },
  
  // 道具配置
  POWERUP: {
    DROP_CHANCE: 0.2,
    BOSS_DROP_COUNT: 3,
    SPEED: 50
  },
  
  // 粒子配置
  PARTICLE: {
    EXPLOSION_COUNT: 20,
    MAX_COUNT: 500
  },
  
  // 输入配置
  INPUT: {
    KEYBOARD: {
      UP: ['ArrowUp', 'KeyW'],
      DOWN: ['ArrowDown', 'KeyS'],
      LEFT: ['ArrowLeft', 'KeyA'],
      RIGHT: ['ArrowRight', 'KeyD'],
      SHOOT: ['Space'],
      ULTIMATE: ['KeyQ'],
      PAUSE: ['KeyP', 'Escape']
    }
  }
};
```

---

## 8. 游戏流程

```
开始 → 主菜单 → 选择模式 → 游戏开始
     ↓
  游戏进行中 → 胜利/失败 → 结算界面 → 返回主菜单
     ↓
  暂停 → 继续游戏/返回主菜单
```

### 8.1 游戏状态机

```javascript
const GAME_STATE = {
  MENU: 'menu',
  PLAYING: 'playing',
  PAUSED: 'paused',
  GAME_OVER: 'game_over',
  VICTORY: 'victory',
  BOSS_WARNING: 'boss_warning'
};
```

---

## 9. 优化建议

### 9.1 性能优化
- 对象池: 子弹、粒子使用对象池复用
- 视口剔除: 只更新和渲染屏幕内的对象
- Canvas分层: 背景、游戏对象、UI分层渲染
- requestAnimationFrame: 使用浏览器原生帧同步

### 9.2 代码优化
- 模块化: 按功能拆分模块
- 继承: 使用ES6类继承减少重复代码
- 配置化: 将常量提取到配置文件
- 事件驱动: 使用事件系统解耦组件

### 9.3 用户体验优化
- 响应式控制: 支持多种输入方式
- 平滑动画: 使用缓动函数
- 视觉反馈: 击中、受伤、升级等效果
- 音效反馈: 各种操作有对应的音效

---

## 10. 扩展功能

### 10.1 计划功能
- [ ] 成就系统
- [ ] 排行榜（本地存储）
- [ ] 皮肤系统
- [ ] 多人对战（WebSocket）
- [ ] 每日任务
- [ ] 商店系统

### 10.2 技术扩展
- [ ] WebGL渲染优化
- [ ] 物理引擎集成
- [ ] AI敌人行为树
- [ ] 关卡编辑器
- [ ] 游戏回放功能

---

## 附录：文件清单

| 文件路径 | 说明 |
|---------|------|
| `index.html` | 主页面 |
| `styles/main.css` | 全局样式 |
| `src/main.js` | 入口文件 |
| `src/game/Game.js` | 游戏核心类 |
| `src/game/Scene.js` | 场景管理 |
| `src/game/Entity.js` | 实体基类 |
| `src/game/Player.js` | 玩家类 |
| `src/game/Enemy.js` | 敌机类 |
| `src/game/Boss.js` | BOSS类 |
| `src/game/Bullet.js` | 子弹类 |
| `src/game/PowerUp.js` | 道具类 |
| `src/game/Particle.js` | 粒子类 |
| `src/ui/HUD.js` | 游戏界面 |
| `src/ui/Menu.js` | 菜单系统 |
| `src/audio/Audio.js` | 音频管理 |
| `src/utils/Collision.js` | 碰撞检测 |
| `src/config/constants.js` | 常量配置 |
| `assets/images/` | 图片资源目录 |
| `assets/sounds/` | 音效资源目录 |