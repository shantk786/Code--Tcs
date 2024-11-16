# 1st question
def minimum_moves(n, instructions):
    # Initialize legs positions
    left_leg, right_leg = "down", "right"
    moves = 0

    # Process each instruction
    for ins in instructions:
        if ins == left_leg or ins == right_leg:
            continue  # No need to move; leg already on the tile
        # Decide which leg to move
        if left_leg != ins and right_leg != ins:
            # Move the leg that minimizes movement
            left_leg, right_leg = right_leg, ins
            moves += 1
        elif left_leg != ins:
            left_leg = ins
            moves += 1
        elif right_leg != ins:
            right_leg = ins
            moves += 1

    return moves

# Input
n = int(input())
instructions = [input().strip() for _ in range(n)]

# Output the result
print(minimum_moves(n, instructions))
