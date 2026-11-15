# Reflection

# 1. In the project I developed further, what did a fresh session get wrong or ask about that an earlier round had already settled? What was missing from the repository that would have prevented it?

The project I developed further is Shooter-Game. In round 1, I decided that I would play test in the browser myself and Claude would not. However, CLAUDE.md still said "play the game in the browser". The round 2 session asked which rule had to be kept, and which one to ignore (this was documented in handoff-round-2, Unresolved 4). This was only fixed in the "Round 3 End" commit. There was also two questions that came back in every handoff from round 2 to 6, it was if whether I had run the play check and whether I approved the control-hint line. These kept coming back because there was nowhere to write the answer down. CLAUDE.md says to not rewrite decisions.md, and another cause is that problem.md calls every round's requirement as "R1". Each handoff warned me that the new R1 might replace the old one so a fresh session cannot tell from the file. What was missing in this whole scenario was just me needing to refine decisions.md and CLAUDE.md, and finalizing those properly so future rounds knew what to do.

# 2. In the round that changed an earlier requirement, what broke? Did the tests catch it, and does the decision record explain the change to someone who was not there?

Nothing in the old code broke. The 39 round 1 tests were not touched since the first build. At the commit 4c03b19, 57 of 58 tests passed and the one failure was the new control-hint test. 12 new tests were red before the code which is the intended order. The tests didn't actually catch the real failure. A shot is created at 'ship.y - SHOT_HEIGHT', so a ship within about 8 px of the top edge loses its shot in the same frame. I reproduced this with the current code, with ship.y at 0 or 5 you get 0 shots, and at 10 you get 1. The round 2 handoff found this and every handoff since then has left it as unresolved. In round 6, this extended to the shotgun bullets as well. The closest test to it was when testing the round 2 addition of being able to move the ship up and down, it calls 'updateGame(state, input, 0)' with dt of 0 and only moves the ship to about y=446 so it did not catch the problem.

The decision records 0001 and 0002 explain their two choices well (diagonal speed and how far up the ship may go). However they do not say what R1 used to be or that this replaces the round 1 R1. Round 1 R1 was that the player can move left and right, Round 2 R1 is that they can move left, right, up, down. 0002 also says that shots still start just above the ship, which is false at the top edge. Someone who wasn't there when the code was written will understand the choices, but not what changed.

# 3. Name one test that caught a real problem, or one test the assistant wrote that checked nothing useful. Quote it.

Yhis is a test that checked nothing useful:
 ==========================
 test("R1: a purple triangle falls at half the speed of a red square", () => {

 assert.equal(game.TRIANGLE_FALL_SPEED, game.ENEMY_SPEED / 2);

 assert.equal(game.TRIANGLE_FALL_SPEED, 50);

 assert.ok(game.TRIANGLE_SIDE_SPEED > 0, "it has a sideways speed");

 });

==========================
game.js defines 'TRIANGLE_FALL_SPEED = ENEMY_SPEED / 2', so the first assertion restates the definition. I changed moveTriangle so that triangles fall at a different speed but this test still passed. Only a different test 'moves down at half speed and sideways at the same time' failed. The round 6 test 'the shotgun cooldown is 10 seconds and a blast has 5 pellets' has the same shape. The test checks two constants and nothing about behavior itself.

# 4. Choose one commit you would now rite differently, and say how.

The commit 4a3ae07 "Round 1 END" is actually the end of round 2. It adds handoff-round-2.md and a block of new rules to CLAUDE.md after the round 2 work. The message is wrong and a later handoff had to explain it. To do this differently, I would make two commits:
"Add round 2 handoff, it includes state, decisions, and warnings for round 3 session"
"Added new rules for tests, decision records, and requirement-ID reuse to CLAUDE.md so later sessions work better and stop re-asking"

# 5. Choose one function in the project I developed further that I did not read while building. Without an assistant open, read and write what I think it does, then ask an assistant and say what I got right or wrong.

This function is from shooter-game, located in game.js

 =========================

 function handleShotHits(state) {

   const enemiesLeft = [...state.enemies];

   const shotsLeft = [];

   for (const shot of state.shots) {

     const hitIndex = enemiesLeft.findIndex((enemy) => rectsOverlap(shot, enemy));

     if (hitIndex === -1) {

       shotsLeft.push(shot);

     } else {

       enemiesLeft.splice(hitIndex, 1);

       state.score += POINTS_PER_ENEMY;

    }

   }

  state.shots = shotsLeft;
   state.enemies = enemiesLeft;
 }

 =========================

What I think it does:
It copies state.enemies and puts it into enemiesLeft while starting shotsLeft. This means that one shot can only kill one enemy and then it is consumed. If the shot does not hit anything then it stays in shotsLeft. I think it uses rectsOverlap to see if any shot hit an enemy.

What the assistant said:
I had the structure right, it is one enemy per shot and missed shots survive and rectsOverlap is the test. I missed that it adds 10 score on each hit, and I also missed that the enemy is taken out of the copy when a shot hits. This means that two shots on one enemy only scores once. It also hits the first overlapping enemy in the array and not the nearest. I was not wrong about what I said but I did not understand how the enemy logic works after a shot is hit.


# Please Read: Submission tests that failed which I have either fixed or cannot be properly fixed
These are the submission tests that failed:

2 - Portfolio has README.md and Reflection.md, and README names the project developed further:
I have since fixed this and listed the project that was developed further.

4 - First commit is only problem.md, with the headings Problem and Initial Requirements:
The commits were fine, the headings were bugged out. I have since fixed the headings however I can not change the initial commits properly.

7a - 5+ rounds, each committed before other work, on 3+ days:
The requirement for this check was that I first commit a new round requirement listed in problem.md, then do the round and then commit it. What I did was do all of that in just one commit in the end. I first made the new requirement in problem.md, then did the entire round and committed all of that in the end. I did however do this over the span of multiple days. I cannot change the earlier commits properly. I have since did a round 7 with the commits done properly, to demonstrate that I can do this properly.

7b - Every requirement has an ID and is named in a test:
Every requirement from round 2 and onwards I just listed as R1 and only after nearing completion of the project I realized this is wrong. This has been fixed, each requirement is labelled properly and files affected by this (tests, etc.) have been properly changed.

7c - A round changes or removes an earlier requirement by ID:
This was actually done but just not labelled properly in the files, I since have labelled it properly.

8 - Vague commit messages:
Some of my very early commits were vague messages. This cannot be changed properly. I have since realized the mistake and committed further commits with good messages.
