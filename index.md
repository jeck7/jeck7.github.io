---
layout: default
---

**Spring rest controller unit test example**


1. How to write correct unit test for rest controllers

 While writing junit test for a rest controller method, we shall keep in mind that:
 
 * A unit test is supposed to test only a certain part of code (i.e. code written in controller class), so we shall mock all the dependencies injected and used in controller class.
 * If the test utilizes other dependencies (e.g. database/network) then it is integration testing and not unit testing.
 * We should not use any webserver otherwise it will make the unit testing slow.
 * Each unit test should be independent of other tests.
 * By definition, unit tests should be fast.
 
2. Unit test controllers using Junit 5 and Mockito
3. Do nothing when possible

