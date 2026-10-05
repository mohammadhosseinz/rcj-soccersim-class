# RCJ SoccerSim Class Examples

This repository contains class-by-class Python examples for learning robot control in the RoboCupJunior Soccer Simulator. It is intended as a companion repository for students who are practicing RCJ SoccerSim concepts such as GPS, compass heading, ball tracking, geometry helpers, team communication, and simple role-based strategy.

The examples are based on the official [RoboCupJunior SoccerSim](https://robocup-junior.github.io/rcj-soccersim/) environment, which uses Webots and Python controllers.

## What is inside?

Each folder represents a lesson or checkpoint in the learning path:

| Folder | Focus |
| --- | --- |
| `class 4 (GPS and compass)` | Reading GPS and compass data from the robot controller. |
| `class 5(class)` | Organizing robot behavior with classes and reusable controller code. |
| `class 6(line,point)` | Basic geometry helpers such as points, lines, and robot movement toward positions. |
| `class 7(team)` | Team-oriented robot code and shared helper logic. |
| `class 8(yellow)` | Blue/yellow team examples with side-aware positioning and strategy code. |
| `class 10` | More complete team behavior, including team packets, role decisions, attack/defense positioning, and shared ball information. |

Most class folders include one or more RCJ SoccerSim controller folders such as:

```text
rcj_soccer_team_blue/
rcj_soccer_team_yellow/
controllers/rcj_soccer_team_blue/
controllers/rcj_soccer_team_yellow/
```

Typical controller files are:

| File | Purpose |
| --- | --- |
| `rcj_soccer_team_blue.py` / `rcj_soccer_team_yellow.py` | Entry point that chooses `robot1.py`, `robot2.py`, or `robot3.py` based on the robot name. |
| `robot1.py`, `robot2.py`, `robot3.py` | Behavior code for each robot. |
| `rcj_soccer_robot.py` | Base helper class for Webots devices, sensors, motors, ball receiver, supervisor receiver, and team communication. |
| `utils.py` | Shared math, geometry, navigation, packet, and strategy helpers. |

## Prerequisites

Install the tools required by the official simulator:

- Python 3.7 or newer on Windows, or Python 3.8 or newer on macOS/Linux.
- Webots. The official SoccerSim documentation currently recommends Webots R2025a as a stable version for the simulator.
- A local copy of the official `rcj-soccersim` project.

Follow the official setup guide here:

- [RCJ SoccerSim Getting Started](https://robocup-junior.github.io/rcj-soccersim/getting_started/)

## How to use these examples

This repository does not include a `.wbt` world file by itself. Use it together with the official RCJ SoccerSim project.

1. Clone or download the official simulator:

   ```bash
   git clone https://github.com/robocup-junior/rcj-soccersim.git
   ```

2. Open the official simulator in Webots:

   ```text
   rcj-soccersim/worlds/soccer.wbt
   ```

3. Pick the class folder you want to test from this repository.

4. Copy the relevant team controller folder into the simulator's `controllers/` directory. For example, to test a blue team from `class 10`, copy:

   ```text
   class 10/rcj_soccer_team_blue
   ```

   into:

   ```text
   rcj-soccersim/controllers/rcj_soccer_team_blue
   ```

5. Run or restart the simulation in Webots.

## Controller structure

In RCJ SoccerSim, each robot runs a Python controller. The team entry file reads the Webots robot name, such as `B1`, `B2`, `B3`, `Y1`, `Y2`, or `Y3`, and starts the matching robot class:

```python
robot = Robot()
name = robot.getName()
robot_number = int(name[1])

if robot_number == 1:
    robot_controller = MyRobot1(robot)
elif robot_number == 2:
    robot_controller = MyRobot2(robot)
else:
    robot_controller = MyRobot3(robot)

robot_controller.run()
```

The official programming guide explains this controller pattern in more detail:

- [How to program your robot](https://robocup-junior.github.io/rcj-soccersim/how_to_robot/)

## Sensors and data used in the examples

The examples build on the standard RCJ SoccerSim robot devices:

- GPS for robot position.
- Compass for heading.
- Four sonar/distance sensors: left, right, front, and back.
- Ball receiver for ball direction and signal strength.
- Supervisor receiver for match/referee data.
- Team emitter and team receiver for communication between robots on the same team.
- Left and right wheel motors for movement.

The base `RCJSoccerRobot` class initializes these devices and exposes helper methods such as:

- `get_gps_coordinates()`
- `get_compass_heading()`
- `get_sonar_values()`
- `is_new_ball_data()`
- `get_new_ball_data()`
- `is_new_team_data()`
- `get_new_team_data()`
- `send_data_to_team(...)`

## Team communication

Later examples use Webots emitters and receivers to share data between robots. The official simulator assigns separate communication channels to the blue and yellow teams, so each robot can receive messages from teammates.

In this repository, team messages are JSON packets that can include:

- robot id
- whether the robot currently sees the ball
- estimated ball distance
- estimated ball position
- robot GPS position
- team decision or role assignment

See the official guide for the simulator's communication model:

- [Inter-robot communication](https://robocup-junior.github.io/rcj-soccersim/communication_between_robots/)

## Running from the command line

For normal learning and debugging, opening `worlds/soccer.wbt` in Webots is the easiest path.

The official simulator can also be run from the command line:

```bash
webots --mode=fast worlds/soccer.wbt
```

For automated matches, recording, Docker usage, and environment variables such as `RCJ_SIM_AUTO_MODE`, `RCJ_SIM_REC_FORMATS`, and `RCJ_SIM_OUTPUT_PATH`, use the official run guide:

- [How to run the simulation](https://robocup-junior.github.io/rcj-soccersim/how_to_run_sim/)

## Suggested learning path

1. Start with `class 4 (GPS and compass)` and confirm that each robot can read its position and heading.
2. Move to `class 5(class)` to understand how the robot code is organized into reusable classes.
3. Study `class 6(line,point)` to practice point, line, distance, and movement calculations.
4. Use `class 7(team)` to begin thinking about team behavior.
5. Compare blue and yellow behavior in `class 8(yellow)` and notice how field side changes affect coordinates and compass logic.
6. Explore `class 10` for a more complete team strategy with shared ball information and role decisions.

## Notes for students

- Keep `robot1.py`, `robot2.py`, and `robot3.py` small when possible. Put repeated math and movement helpers in `utils.py`.
- Always call `self.robot.step(TIME_STEP)` inside the main loop.
- Check whether new ball, team, or supervisor data exists before reading from a receiver.
- Clamp motor speeds before sending them to the wheels.
- Print sensor values while learning, but remove or reduce noisy debug output before running full matches.
- If a robot behaves differently on the blue and yellow sides, check coordinate conversion and compass normalization first.

## Useful links

- [Official RCJ SoccerSim documentation](https://robocup-junior.github.io/rcj-soccersim/)
- [Getting Started](https://robocup-junior.github.io/rcj-soccersim/getting_started/)
- [How to program your robot](https://robocup-junior.github.io/rcj-soccersim/how_to_robot/)
- [Inter-robot communication](https://robocup-junior.github.io/rcj-soccersim/communication_between_robots/)
- [How to run the simulation](https://robocup-junior.github.io/rcj-soccersim/how_to_run_sim/)
