# PROJECT STRUCTURE DETAIL (TREE)

Tài liệu này mô tả chức năng của thư mục/file theo dạng cây cho toàn bộ project `OOP`.

> Lưu ý: Repo có 5424 file trong `assets/` (chủ yếu là ảnh/sound). Để tài liệu vẫn dễ đọc, phần code và cấu hình được mô tả tới cấp file; phần asset rất lớn được mô tả theo gói/folder và các file đại diện.

## 1) Tổng quan root

```text
OOP/
|-- .editorconfig                         # Quy tắc format cơ bản cho editor
|-- .gitattributes                        # Cấu hình line-ending/thuộc tính Git
|-- .gitignore                            # Danh sách file/folder bỏ qua khi commit
|-- Bugs.md                               # Nơi các thành viên ghi lỗi trong quá trình test
|-- README.md                             # Hướng dẫn cài đặt, compile, run, debug hotkeys
|-- build.gradle                          # Cấu hình Gradle cha + task generateAssetList
|-- settings.gradle                       # Khai báo module: core, lwjgl3
|-- gradle.properties                     # Version thư viện + thông số Gradle/JVM
|-- gradlew / gradlew.bat                 # Gradle wrapper script (Linux/macOS và Windows)
|-- gradle/
|   |-- gradle-daemon-jvm.properties      # URL toolchain JDK 17 cho daemon (foojay)
|   `-- wrapper/
|       |-- gradle-wrapper.jar            # Binary Gradle Wrapper
|       `-- gradle-wrapper.properties     # Version Gradle wrapper (8.14)
|-- core/                                 # Module game logic chính (LibGDX core)
|-- lwjgl3/                               # Module desktop launcher (LWJGL3)
|-- assets/                               # Toàn bộ tài nguyên game (ảnh, font, nhạc, map...)
|-- build/                                # Thư mục build tạo ra bởi Gradle
|-- .gradle/                              # Cache/build metadata cục bộ của Gradle
|-- .idea/                                # Cấu hình IDE (IntelliJ)
`-- .git/                                 # Metadata repository Git
```

## 2) Module `core` (chi tiết tới cấp file)

```text
core/
|-- build.gradle                          # Dependency cho core: gdx, gdx-ai, box2d, freetype...
`-- src/main/java/com/paradise_seeker/game/
    |-- main/
    |   `-- Main.java                     # Lớp game gốc; khởi tạo tài nguyên, screen manager, vòng đời game
    |
    |-- screen/
    |   |-- MainMenuScreen.java           # Màn hình menu chính
    |   |-- GameScreen.java               # Màn hình gameplay chính
    |   |-- PauseScreen.java              # Màn hình tạm dừng
    |   |-- SettingScreen.java            # Màn hình cài đặt
    |   |-- InventoryScreen.java          # Màn hình inventory
    |   |-- DeadScreen.java               # Màn hình khi player chết
    |   |-- WinScreen.java                # Màn hình chiến thắng/kết thúc màn
    |   |-- ControlScreen.java            # Màn hình hướng dẫn điều khiển
    |   `-- cutscene/
    |       |-- CutScene.java             # Base class cho cutscene
    |       |-- IntroCutScene.java        # Cutscene mở đầu
    |       |-- EndMap1.java              # Cutscene kết map 1
    |       |-- EndMap2.java              # Cutscene kết map 2
    |       |-- EndMap3.java              # Cutscene kết map 3
    |       |-- EndMap4.java              # Cutscene kết map 4
    |       `-- EndGame.java              # Cutscene ending tổng
    |
    |-- map/
    |   |-- GameMap.java                  # Abstraction/base map
    |   |-- GameMap1.java                 # Logic/map data cho map 1
    |   |-- GameMap2.java                 # Logic/map data cho map 2
    |   |-- GameMap3.java                 # Logic/map data cho map 3
    |   |-- GameMap4.java                 # Logic/map data cho map 4
    |   |-- GameMap5.java                 # Logic/map data cho map 5
    |   |-- GameMapManager.java           # Quản lý chuyển map, map hiện tại
    |   |-- MonsterFactory.java           # Spawn/quản lý tạo quái theo map
    |   `-- SolidObject.java              # Đối tượng vật cản và collision trên map
    |
    |-- entity/
    |   |-- Character.java                # Base class cho các nhân vật/thực thể sống
    |   |
    |   |-- player/
    |   |   |-- Player.java               # Player entity: di chuyển, combat, tương tác
    |   |   |-- status/
    |   |   |   `-- PlayerStatusManager.java        # Quản lý HP/MP/stats/trạng thái player
    |   |   |-- input/
    |   |   |   |-- PlayerInputHandler.java         # Xử lý input đơn lẻ cho player
    |   |   |   `-- PlayerInputHandlerManager.java  # Điều phối nhiều input handler
    |   |   |-- inventory/
    |   |   |   |-- Inventory.java                 # Cấu trúc dữ liệu inventory
    |   |   |   `-- PlayerInventoryManager.java    # Logic thêm/xóa/sử dụng item
    |   |   `-- skill/
    |   |       |-- Skill.java                     # Interface/base skill
    |   |       |-- PlayerSkill.java               # Base skill của player
    |   |       |-- PlayerSkill1.java              # Kỹ năng 1
    |   |       |-- PlayerSkill2.java              # Kỹ năng 2
    |   |       |-- skillanimation/
    |   |       |   |-- SkillAnimationManager.java  # Quản lý animation của skill
    |   |       |   |-- PlayerSkill1Animation.java  # Animation cho skill 1
    |   |       |   `-- PlayerSkill2Animation.java  # Animation cho skill 2
    |   |       `-- skillrender/
    |   |           |-- SkillRenderer.java         # Base renderer cho skill
    |   |           |-- Skill1Renderer.java        # Renderer skill 1
    |   |           `-- Skill2Renderer.java        # Renderer skill 2
    |   |
    |   |-- npc/
    |   |   |-- NPC.java                           # Base NPC
    |   |   |-- dialogue/
    |   |   |   |-- NPCDialogue.java               # Đơn vị hội thoại
    |   |   |   `-- NPCDialogueManager.java        # Quản lý flow hội thoại NPC
    |   |   `-- gipsy/
    |   |       |-- Gipsy.java                     # NPC Gipsy cụ thể
    |   |       `-- GipsyStateManager.java         # Trạng thái/chuỗi sự kiện của Gipsy
    |   |
    |   `-- monster/
    |       |-- Monster.java                       # Base class cho quái
    |       |-- status/
    |       |   `-- MonsterStatusManger.java       # Quản lý stat/trạng thái quái
    |       |-- ai/
    |       |   |-- AI.java                        # Interface/lớp AI cơ sở
    |       |   `-- MonsterAI.java                 # AI điều khiển hành vi quái
    |       |-- projectile/
    |       |   |-- HasProjectiles.java            # Contract cho quái có bắn đạn/skill tầm xa
    |       |   `-- MonsterProjectile.java         # Đạn/skill projectile của quái
    |       |-- creep/
    |       |   |-- CyanBat.java                   # Quái creep CyanBat
    |       |   |-- DevilCreep.java                # Quái creep DevilCreep
    |       |   |-- EvilPlant.java                 # Quái creep EvilPlant
    |       |   |-- FlyingCreep.java               # Quái creep bay
    |       |   |-- GhostStatic.java               # Quái creep dạng ma/immobile pattern
    |       |   |-- RatCreep.java                  # Quái creep chuột
    |       |   |-- SkeletonEnemy.java             # Quái creep xương
    |       |   `-- YellowBat.java                 # Quái creep YellowBat
    |       |-- elite/
    |       |   |-- FirewormElite.java             # Quái elite Fireworm
    |       |   |-- FlyingDemon.java               # Quái elite FlyingDemon
    |       |   |-- IceElite.java                  # Quái elite hệ băng
    |       |   |-- MinotaurElite.java             # Quái elite Minotaur
    |       |   `-- Necromancer.java               # Quái elite Necromancer
    |       `-- boss/
    |           |-- CyclopBoss.java                # Boss Cyclop
    |           |-- FireDemon.java                 # Boss Fire Demon
    |           |-- Nyx.java                       # Boss Nyx
    |           `-- ParadiseKing.java              # Final boss Paradise King
    |
    |-- rendering/
    |   |-- Renderable.java                        # Interface cho đối tượng có thể render
    |   |-- MonsterHPBarManager.java               # Vẽ/quản lý HP bar quái
    |   |-- animations/
    |   |   |-- AnimationManager.java              # Base animation manager
    |   |   |-- PlayerAnimationManager.java        # Animation player
    |   |   |-- NPCAnimationManager.java           # Animation NPC
    |   |   `-- MonsterAnimationManager.java       # Animation monster
    |   |-- effects/
    |   |   |-- DashTrail.java                     # Đối tượng hiệu ứng vệt dash
    |   |   `-- DashTrailManager.java              # Quản lý hiệu ứng dash trail
    |   `-- renderer/
    |       |-- playerrender/
    |       |   |-- PlayerRenderer.java            # Renderer player
    |       |   `-- PlayerRendererManager.java     # Quản lý renderer player
    |       |-- npcrender/
    |       |   |-- NPCRenderer.java               # Renderer NPC
    |       |   `-- NPCRendererManager.java        # Quản lý renderer NPC
    |       `-- monsterrender/
    |           |-- MonsterRenderer.java           # Renderer monster
    |           `-- MonsterRendererManager.java    # Quản lý renderer monster
    |
    |-- collision/
    |   |-- Collidable.java                        # Contract cho đối tượng tham gia va chạm
    |   |-- CollisionSystem.java                   # Hệ thống kiểm tra/giải quyết va chạm tổng
    |   `-- CombatCollisionHandler.java            # Xử lý va chạm liên quan combat
    |
    |-- object/
    |   |-- GameObject.java                        # Base object trên map
    |   |-- Chest.java                             # Vật thể rương
    |   |-- Portal.java                            # Vật thể cổng dịch chuyển/chuyển map
    |   |-- Book.java                              # Vật thể sách (story/interaction)
    |   `-- item/
    |       |-- Item.java                          # Base class cho item
    |       |-- HPPotion.java                      # Potion hồi máu
    |       |-- MPPotion.java                      # Potion hồi mana
    |       |-- ATKPotion.java                     # Potion buff sát thương
    |       |-- Skill1Potion.java                  # Item liên quan skill 1
    |       |-- Skill2Potion.java                  # Item liên quan skill 2
    |       `-- Fragment.java                      # Mảnh key/collectible để mở khóa
    |
    |-- ui/
    |   |-- HUD.java                               # Head-up display khi chơi
    |   `-- DialogueBox.java                       # Khung hiển thị hội thoại
    |
    |-- story/
    |   |-- RouteType.java                         # Enum route/nhánh cốt truyện
    |   |-- StoryStateManager.java                 # Lưu và cập nhật trạng thái cốt truyện
    |   |-- RouteResolver.java                     # Xác định route hiện tại theo điều kiện
    |   `-- EndingResolver.java                    # Quy tắc chọn ending
    |
    |-- save/
    |   `-- SaveManager.java                       # Lưu/tải trạng thái game
    |
    |-- debug/
    |   `-- DebugHotkeys.java                      # F1/F2/... hotkeys phục vụ test nhanh
    |
    |-- interaction/
    |   `-- InteractionState.java                  # Trạng thái tương tác (NPC/object/dialog...)
    |
    `-- meta/
        `-- MetaDetector.java                      # Kiểm tra môi trường/chạy trong dev mode
```

## 3) Module `lwjgl3` (desktop launcher)

```text
lwjgl3/
|-- build.gradle                                  # Cấu hình app desktop, run, jar, đóng gói đa nền tảng
|-- nativeimage.gradle                            # Cấu hình build GraalVM native-image (tùy chọn)
|-- icons/
|   |-- logo.png                                  # Icon chính
|   |-- logo.ico                                  # Icon Windows
|   `-- logo.icns                                 # Icon macOS
`-- src/main/
    |-- java/com/paradise_seeker/game/lwjgl3/
    |   |-- Lwjgl3Launcher.java                   # Entry point desktop
    |   `-- StartupHelper.java                    # Hỗ trợ startup tương thích theo OS
    `-- resources/
        `-- META-INF/native-image/...             # Tài nguyên cho native-image (nếu dùng)
```

## 4) Thư mục `assets` (chi tiết theo nhóm chức năng)

```text
assets/                                           # Tài nguyên runtime game
|-- assets.txt                                    # Danh sách file asset tự động sinh bởi Gradle task
|-- cutscene/
|   |-- Chapter 1/ ... Chapter 5/                 # Ảnh cutscene từng chương
|   `-- Ending/                                   # Ảnh ending
|-- fonts/                                        # Font dùng cho UI/text
|   |-- m5x7.ttf
|   |-- MinecraftStandard*.otf
|   |-- Purisa Bold.ttf
|   `-- x12y16pxMaruMonica.ttf
|-- music/
|   |-- menutheme.mp3                             # Nhạc menu
|   |-- map1.mp3 ... map5.mp3                     # Nhạc theo từng map
|   `-- music.mp3                                 # Track chung/thử nghiệm
|-- tilemaps/
|   `-- TileMaps/
|       |-- maps/                                 # File map (tmx/json theo bộ tile)
|       |-- map1-tilesets/ ... map5-tilesets/     # Tileset theo map
|       |-- project.tiled-project                 # Cấu hình dự án Tiled
|       |-- project.tiled-session                 # Session editor Tiled
|       `-- testmap.tiled-session                 # Session test map
|-- menu/
|   |-- start_menu/main_menu/                     # Sprite menu bắt đầu
|   |-- settings_menu/setting_main/               # Sprite menu cài đặt
|   |-- inventory_menu/                           # Nền/khung inventory screen
|   |-- end_menu/main_death/                      # Sprite game over/dead
|   `-- win_menu/menu_end/                        # Sprite màn hình win
|-- ui/
|   |-- HUD/                                      # HP/MP bar, nút pause/inventory, skill slot
|   |-- dialog/                                   # Khung hội thoại + mũi tên
|   `-- HP_bar_monster/hpm/                       # Frame HP bar cho quái
|-- items/
|   |-- potion/                                   # Sprite potion HP/MP
|   |-- atkbuff_potion/                           # Sprite potion buff attack
|   |-- buff/                                     # Icon/trạng thái buff
|   |-- fragment/                                 # Sprite mảnh key
|   |-- inventory_icons/                          # Icon item trong inventory
|   |-- menu_inventory/                           # Texture UI inventory chi tiết
|   |-- book.png                                  # Sprite sách
|   `-- fragment3.png                             # Sprite fragment cụ thể
|-- images/
|   |-- Entity/                                   # Sprite player/NPC/monster/skills (rất lớn)
|   |   |-- characters/                           # Toàn bộ animation sprite character
|   |   `-- skills/                               # Hiệu ứng và sprite skill entity
|   |-- objects/chest/                            # Sprite rương
|   |-- objects/portal/                           # Sprite cổng dịch chuyển
|   `-- spritesheet_smoke.png                     # Sprite sheet smoke effect
`-- GameGraphics/assetpack/                       # Kho asset pack nguồn (third-party + tự tạo)
    |-- [FREE] Mini Dungeon Tileset/              # Tile dungeon mini free
    |-- 0x72_DungeonTilesetII_v1.7/               # Tileset dungeon 0x72
    |-- 16x16 dungeon ii wall reconfig/           # Biến thể wall tileset
    |-- Basic Asset Pack/                         # Gói asset cơ bản 1
    |-- basic asset pack2/                        # Gói asset cơ bản 2
    |-- Basic Asset Pack3/                        # Gói asset cơ bản 3
    |-- Blue Witch/                               # Sprite nhân vật Blue Witch
    |-- char free/                                # Sprite character free
    |-- Dark VFX 01 - 02/                         # VFX tối/magic effect
    |-- Enemy_Animations_Set/                     # Animation enemy set
    |-- fire_fx_v1.0/                             # Hiệu ứng lửa
    |-- FREE Mana Seed Character Base Demo/       # Character base demo + guide
    |-- Free Paper UI System/                     # Gói UI rất lớn (nhiều component)
    |-- Free Pixel Effects Pack/                  # Pixel VFX pack
    |-- FreeUi/                                   # UI pack bổ sung
    |-- Hana Caraka - Base Character [sample]/    # Mẫu base character
    |-- Minifantasy_ForgottenPlains_v3.5_Free_Version/ # Tileset/asset fantasy
    |-- MinifolksHumans/                          # Sprite people/humans
    |-- MinifolksVillagers/                       # Sprite villagers
    |-- New_All_Fire_Bullet_Pixel_16x16/          # Đạn/lửa pixel 16x16
    |-- Pixel Art Top Down - Basic v1.1.2/        # Top-down pack
    |-- RF_Catacombs_v1.0/                        # Catacombs tileset/props
    |-- Retro Inventory/                          # Asset inventory UI retro
    |-- Top_Down_Adventure_Pack_v.1.0/            # Gói top-down adventure
    |-- ase1/ ase2/                               # Folder xử lý asset trung gian
    |-- nature free/                              # Asset nature free + license
    |-- tf_darkdimension_updated2020/             # Asset dark dimension
    `-- throneroom1.png                           # Ảnh phòng ngai vàng
```

## 5) Thư mục hệ thống/IDE/build

```text
.git/                                             # Lịch sử commit, refs, objects
.gradle/                                          # Cache Gradle cục bộ (không phải source game)
.idea/                                            # Cấu hình project cho IntelliJ
build/reports/problems/problems-report.html       # Báo cáo vấn đề build do Gradle sinh
```

## 6) Ghi chú phạm vi chi tiết

- Phần code (`core`, `lwjgl3`) đã mô tả từng file Java và vai trò chính.
- Phần assets quá lớn (622 folder, 5424 file) nên được mô tả theo nhóm/folder chức năng; các tên file được liệt kê đại diện ở các khu vực quan trọng (`cutscene`, `music`, `fonts`, `items`, `menu`).
- Nếu bạn muốn, mình có thể tạo tiếp **bản mở rộng full-file assets** (liệt kê toàn bộ 5424 file trong tree + mô tả theo pattern tên file).

