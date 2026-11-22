# Ms. Pac-Man with Deep RL: DDQN and Dueling DDQN + two modifications

Code for the project page: https://sabbiracoustic1006.github.io/projects/ms-pacman-rl/
Full report: https://sabbiracoustic1006.github.io/projects/ms-pacman-rl/report.pdf

The agent learns Ms. Pac-Man (`MsPacman-v0`, OpenAI Gym) from pixels: grayscale 84×84 frames, 4 frames stacked.

Two changes to the standard baselines:

1. **Reward modification (`--reward_mod`).** The step reward is -1 when nothing is eaten, and -200 when a life is lost. The baseline uses 0 for both.
2. **Ghost-state auxiliary task (the `*_mod2` files).** Two extra heads share the CNN features with the Q-head:
   - one predicts whether the ghosts are scared (2 classes)
   - one predicts how many power pills are left (5 classes: 0–4)

   Labels come for free from the reward signal: eating a power pill gives reward 50 and starts a 40-step "scared" window. See `get_env_state` in `utils.py`. Loss: `L = L_DDQN + α · (CE(power pills) + CE(ghost state))`, with label smoothing 0.2.

## Layout

```
DDQN/
  network.py         DeepQNetwork (baseline) and DeepQNetworkMod (Q + 2 auxiliary heads)
  ddqn.py            DDQN agent (online + target network, replay buffer)
  ddqn_mod2.py       DDQN agent with the ghost-state auxiliary loss
  run_ddqn.py        train / evaluate baseline or reward-modified DDQN
  run_ddqn_mod2.py   train / evaluate DDQN with the auxiliary task
  buffer.py          replay buffer (stores the auxiliary labels too)
  utils.py           Gym wrappers (frame skip 4, grayscale, resize, stack), reward modification, label extraction
  get_plots.py       running-average training curves
Dueling DDQN/        the same files for Dueling DDQN
```

## Run

```bash
pip install -r requirements.txt

cd DDQN
python run_ddqn.py --name_exp DDQN_baseline                 # baseline
python run_ddqn.py --name_exp DDQN_Reward_Mod --reward_mod  # modification 1
python run_ddqn_mod2.py --name_exp DDQN_Mod2 --alpha 0.5    # modification 2
python run_ddqn.py --name_exp DDQN_baseline --eval          # evaluate a saved agent

cd "../Dueling DDQN"
python run_dueling_ddqn.py --name_exp DuelDDQN_baseline
python run_dueling_ddqn_mod2.py --name_exp DuelDDQN_Mod2 --alpha 0.25
```

Defaults: 5,000 episodes, batch size 64, learning rate 1e-4, γ = 0.99, target-network copy every 10,000 steps, replay memory 50,000, ε from 1.0 to 0.1 (decay 5e-6). Trained weights are not included.
