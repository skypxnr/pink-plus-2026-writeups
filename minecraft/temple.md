
# Temple

| Points | Category  | Author  | Stats          |
|--------|-----------|---------|----------------|
| 25     | Minecraft | QWERTZ  | 3 solves / 2 likes |

## Description

The Sculkers conquered my beautiful temple. They want to destroy the holy flag, which is only known to the original residents of the temple.

**Server:** `mc.pink.bayern`

## Walkthrough

### 1. Enter the temple

Right-click the **Temple NPC** shown on the map to enter the temple. You will be teleported to the temple area.

![Temple NPC on the map](https://github.com/user-attachments/assets/cce7122f-1274-4543-988b-6f1c49de34a6)

![Teleported to the temple](https://github.com/user-attachments/assets/efec70fa-e7f3-4e6a-82ab-748f6cb9f60b)

### 2. Talk to the guard

Right-click the guard standing at the entrance. This produces the following message:

![Guard message](https://github.com/user-attachments/assets/59b34732-69f4-401c-8c79-266e8f08f7f7)

### 3. Find the climber

Go to the location indicated by the guard. There you will find a **climber** who gives you a hint about where to go next.

### 4. Find the girl

Continue to the next area. A **girl hiding in the corner** is there.

![Girl hiding in the corner](https://github.com/user-attachments/assets/5b7d3b75-3d2d-47a8-a956-8eda6ae51cbb)

### 5. Interact with the girl

Interacting with her gives you several messages, including a piece of rotated text:

![Girl's messages](https://github.com/user-attachments/assets/1a71d0c9-1f41-442a-9ce1-71113797f50b)

```
SLQN{0_W3pso3_0i_G4unq3vv_exuqv_o1n3_ilu3}
```

### 6. Decode the text

The text is encrypted with **ROT23**. Shifting each letter forward by 23 positions in the alphabet (wrapping around after Z) produces the flag. Digits and symbols are left unchanged.

## Flag

```
PINK{0_T3mpl3_0f_D4rkn3ss_burns_l1k3_fir3}
```

