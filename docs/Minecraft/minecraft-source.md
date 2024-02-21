## ZombieEntity

ZombieEntity是Minecraft游戏中的僵尸实体类，继承自HostileEntity（敌对实体基类）。该类中定义了大量属性、方法和内部类，主要用于控制僵尸的行为特性、AI目标以及与游戏世界的交互。

- 定义了一系列静态变量和数据追踪器（DataTracker），如BABY表示僵尸是否为婴儿形态，CONVERTING_IN_WATER表示僵尸是否在水中转化状态等。
- 实现了多种AI目标（Goal），例如寻找并攻击玩家、村民等生物，破坏门，漫游，寻找路径穿越村庄等。
- 提供了初始化僵尸属性的方法createZombieAttributes()，设置僵尸的基础属性如移动速度、攻击力等。
- 控制僵尸能否破坏门的逻辑，并能转换成其他类型的实体，如溺尸（Drowned）或僵尸村民（ZombieVillagerEntity）。
- 包含处理僵尸在日光下燃烧、受伤声音播放、生成时装备物品、繁殖机制、难度影响下的行为变化等功能。
- 实现了读写NBT标签的方法，以便存储和加载僵尸实体的各种状态信息。
- 定义了一个DestroyEggGoal内部类，用于实现僵尸踩碎海龟蛋的行为。 
  
总之，ZombieEntity类全面涵盖了僵尸在游戏中可能出现的所有行为特征和生命周期管理。


https://cdn.midjourney.com/61192e90-0ff6-46ad-951c-31a450e91ff5/0_3.png




## fabric开发模组，创建一个僵尸，设置营地，并自动寻找周围其他营地的僵尸攻击

```java
package cn.org.porter.entity;

import net.minecraft.command.argument.EntityAnchorArgumentType;
import net.minecraft.entity.Entity;
import net.minecraft.entity.EntityType;
import net.minecraft.entity.LivingEntity;
import net.minecraft.entity.ai.goal.Goal;
import net.minecraft.entity.mob.ZombieEntity;
import net.minecraft.server.world.ServerWorld;
import net.minecraft.util.math.Box;
import net.minecraft.util.math.Vec3d;
import net.minecraft.world.World;

import java.util.EnumSet;
import java.util.List;

public class CampZombieEntity extends ZombieEntity {
    private String camp;

    public CampZombieEntity(EntityType<? extends ZombieEntity> entityType, World world) {
        super(entityType, world);
    }

    // 设置或获取营地的方法
    public void setCamp(String camp) {
        this.camp = camp;
    }

    public String getCamp() {
        return this.camp;
    }


    public static class AttackDifferentCampZombieGoal extends Goal {
        private final CampZombieEntity zombie;

        public AttackDifferentCampZombieGoal(CampZombieEntity zombie) {
            this.zombie = zombie;
            this.setControls(EnumSet.of(Control.TARGET));
        }

        @Override
        public boolean canStart() {
            return this.zombie.getTarget() == null || !this.isSuitableTarget(this.zombie.getTarget());
        }

        // ... 其他目标选择器方法 ...

        private boolean isSuitableTarget(Entity entity) {
            if (entity instanceof LivingEntity) {
                return ((LivingEntity) entity).isAlive() && entity instanceof CampZombieEntity && !((CampZombieEntity) entity).getCamp().equals(this.zombie.getCamp());
            }
            return false;
        }

        private void findNewTarget() {
            ServerWorld serverWorld = (ServerWorld) this.zombie.getEntityWorld();
            Box box = this.zombie.getBoundingBox().expand(8.0D);
            List<LivingEntity> targets = serverWorld.getEntitiesByClass(LivingEntity.class, box, e -> e instanceof CampZombieEntity && !((CampZombieEntity) e).getCamp().equals(this.zombie.getCamp()));
            if (!targets.isEmpty()) {
                LivingEntity target = targets.get(0);
                if (target instanceof ZombieEntity) { // 确保目标为僵尸实体
                    this.zombie.setTarget(target);
                }
            }
        }


        @Override
        public void tick() {
            LivingEntity target = this.zombie.getTarget();
            if (target instanceof LivingEntity && !((LivingEntity) target).isAlive()) {
                this.zombie.setTarget(null);
            } else if (target == null || !this.isSuitableTarget(target)) {
                this.findNewTarget();
            } else {
                ((ServerWorld) this.zombie.getEntityWorld()).getProfiler().push("attack");
                this.zombie.getNavigation().startMovingTo(target, 1.0D);

                Vec3d lookVec = new Vec3d(target.getX(), target.getEyeY(), target.getZ()); // 创建一个Vec3d实例表示目标的位置
                this.zombie.lookAt(EntityAnchorArgumentType.EntityAnchor.EYES, lookVec); // 使用正确的lookAt方法和Vec3d参数

                ((ServerWorld) this.zombie.getEntityWorld()).getProfiler().pop();
            }
        }


        // ... 其他方法 ...
    }
    // ... 其他初始化方法 ...
}

    // 注册营地僵尸
    public static final EntityType<CampZombieEntity> CUBE = Registry.register(
            Registries.ENTITY_TYPE,
            new Identifier(MOD_ID, "CampZombie"),
            FabricEntityTypeBuilder.create(SpawnGroup.CREATURE, CampZombieEntity::new).dimensions(EntityDimensions.fixed(0.75f, 0.75f)).build()
    );

```



fabric mod 开发， 要求 如下：
初始化世界大小为40*40 ,进入的玩家随机分配一块 4*4的阵地,不同玩家的阵地不能重叠,不同玩家的阵地不能超过世界大小,不同玩家的阵地颜色不同使用透明的颜色圈起来
玩家可以移动,可以强占其他玩家阵地,抢占的阵地颜色改变,圈起来的范围改变,玩家可以自动攻击其他玩家.






