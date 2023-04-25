# Pairing

## Standards

- Describe 2 techniques for pair programming

## Lesson

Pair Programming is an important part of the [Garage Method](https://www.ibm.com/garage/method/practices/code/practice_pair_programming). We will use pair programming throughout the Project Portion of the course and intermittently before that.

What is pair programming?

> Two humans, working on a single problem, using two keyboards, two mice, and two monitors.

This concept extends beyond _collaboration_ and into _joint problem-solving_. Developers who use this practice find that they now have another person present who will hold them accountable for "doing the right thing."

One goal of pair programming is to have the feeling of "inline code review" with the outcome being higher quality software.

### Roles

There are two roles in pair programming:

- _Driver_ - The Driver is in command of the machine. The driver is focussed on completing the tiny goal at hand, ignoring larger issues for the moment. A driver should always talk through what they are doing while doing it.
- _Navigator_ - The Navigator is watching out for big picture issues while remaining engaged in the implementation. The Navigator facilitates inline code reviews by asking the Driver about their implementation choices or suggesting potential alternatives.

The idea of this role division is to have two different perspectives on the code. The driver's thinking is supposed to be more tactical, thinking about the details, the lines of code at hand. The navigator can think more strategically in their observing role. They have the big picture in mind.

### Techniques

There are a few key pair programming techniques that you may decide to use:

- _Ping-Pong Pairing_ - Developer A writes a failing test, Developer B writes the minimum amount of code possible to make the test pass, then Developer B writes a failing test. Developer A writing the minimum amount of code possible to make the test pass, then Developer A writes a failing test. And on and on until there are no more tests to write.
- _Pomodoro_ - This technique uses the basic principle behind the [Pomodoro Technique](https://en.wikipedia.org/wiki/Pomodoro_Technique#:~:text=The%20Pomodoro%20Technique%20is%20a,length%2C%20separated%20by%20short%20breaks.). Developers work in alternating time periods with one serving as driver and the other as navigator, strictly switching on a time basis.
- _Ensemble Programming_ - Formerly known as _Mob Programming_ This involves programming in a group larger than two, typically with a single person acting as a "Code Scribe" who implements the programming decisions the team makes.

Pair programming is one technique the Garage Method uses to promote teamwork, communication, and, ultimately, deliver high-quality, low-bug software.

You can use [this pairing rubric](./pairing-rubric.md) to help evaluate how well you are pairing.
