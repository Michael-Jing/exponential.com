+++
title=  "Threads in Python"
date=   2020-08-09 20:31:00
+++


# Threads in Python

## Types of OS threads

- POSIX
- Windows

## user threads models

- 1 - 1
- M - 1
- M - N

## States

- New
- Runnable
- Running

- Not runnable
- Dead

## Create threads

- threading.Thread
- Inherit Thread class
- fork

## Apis
- setDaemon(True)
- threading.current_thread()
- threading.main_thread()
- threading.enumerate()
- threading.active_count()

## Gracefully terminate
- you can not, process has terminate() method
