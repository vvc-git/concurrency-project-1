# INE5410 - Assignment 1 - 2021-1

This repository contains the base code for Assignment 1 of INE5410 from semester 2021-1.

## Problem Statement

![WhatsApp Image 2024-07-23 at 11 36 30](https://github.com/user-attachments/assets/d08d1c41-eb7d-48ec-af32-d3254cf6cea9)

## Solution

# README

## Viking Synchronization

### Implementation Table

| Function/Component            | Description                                                                                     |
|------------------------------|-----------------------------------------------------------------------------------------------|
| **Table Buffer**           | Uses a buffer (`buffer_mesa`) to represent the table. Each position can have three values: |
|                              | - `-1`: Empty space                                                                          |
|                              | - `0`: Normal Viking                                                                          |
|                              | - `1`: Berserker Viking                                                                       |
| **Mutexes**                  | Control access to the buffer to ensure multiple vikings don't modify the value simultaneously. |
|                              | Prevent buffer changes during seat availability checking.                 |

### Function `adquire_seats_plates`

| Situation                     | Description                                                                                     |
|------------------------------|-----------------------------------------------------------------------------------------------|
| **Table with one chair**     | - Locks the mutex at position 0.                                                                 |
|                              | - Checks if the position is empty and fills it with `1` or `0`, depending on the viking type.    |
| **Table with multiple chairs** | - A loop attempts to lock a chair by checking `trylock(table_position) == 0`.                |
|                              | - Calls the helper function `position_available` which returns `1` if the position is valid, or `0` if not. |
|                              | - If the position is valid, the buffer is updated, and the `get_plate` function is called to obtain two plates. |
|                              | - The mutex `search_control` is unlocked, and the chair position is returned.                 |

### Waiting for Banquet to End

| Component                   | Description                                                                                     |
|------------------------------|-----------------------------------------------------------------------------------------------|
| **Semaphore and Mutex**         | - `finish_eating`: Semaphore to wait for the end of the banquet.                                   |
|                              | - `food_organizer`: Mutex to synchronize the `total_eaten` counter.                           |
| **Counter**                 | - Starts at zero and is incremented every time a viking leaves their seat.                      |
|                              | - Checks if all vikings have finished eating (`total_eaten == horde_size`).            |
|                              | - Releases the `finish_eating` semaphore with `2*horde_size` posts to allow all vikings to make their prayers. |

### Choosing Gods for Prayers

| Component                   | Description                                                                                     |
|------------------------------|-----------------------------------------------------------------------------------------------|
| **Semaphores**                | Used to limit the number of prayers to each god, ensuring rival gods don't receive an unequal number of prayers. |
| **Function `chieftain_get_god`** | - Generates a random number to select a god.                                           |
|                              | - Checks if the god's semaphore has available space (`sem_trywait(semaphore[num_rand]) == 0`).         |
|                              | - If yes, pos
