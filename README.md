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



#2nd question 

def max_words_fit(k, words, n, m):
    lines_used = 1  # Start with the first line
    current_line_length = 0  # Characters used in the current line
    words_fit = 0  # Count of words placed

    print(f"Debug: Total words = {k}, Max lines = {n}, Max line length = {m}")
    for word in words:
        word_length = len(word)

        print(f"Debug: Processing word '{word}', word length = {word_length}")
        # Case 1: If current line is empty, place the word directly
        if current_line_length == 0:
            if word_length <= m:
                current_line_length = word_length
                words_fit += 1
                print(f"Debug: Placed word '{word}' in new line (current_line_length={current_line_length})")
            else:
                break  # Word too long to fit in any line

        # Case 2: If the word fits in the current line (including space), add it
        elif current_line_length + 1 + word_length <= m:
            current_line_length += 1 + word_length  # Account for the space
            words_fit += 1
            print(f"Debug: Placed word '{word}' in current line (current_line_length={current_line_length})")

        # Case 3: If the word doesn't fit, move to the next line
        else:
            lines_used += 1  # Increment line count
            print(f"Debug: Moving to next line (lines_used={lines_used})")
            if lines_used > n:  # If all lines are used, stop
                break
            # Place the word in the new line
            if word_length <= m:
                current_line_length = word_length
                words_fit += 1
                print(f"Debug: Placed word '{word}' in new line (current_line_length={current_line_length})")
            else:
                break  # Word too long to fit in any line

    return words_fit  # Return the total number of words placed
# 3rd Question 


# Define the seven segment representations
seven_segment_map = {
    "0": [" _ ", "| |", "|_|"],
    "1": ["   ", "  |", "  |"],
    "2": [" _ ", " _|", "|_ "],
    "3": [" _ ", " _|", " _|"],
    "4": ["   ", "|_|", "  |"],
    "5": [" _ ", "|_ ", " _|"],
    "6": [" _ ", "|_ ", "|_|"],
    "7": [" _ ", "  |", "  |"],
    "8": [" _ ", "|_|", "|_|"],
    "9": [" _ ", "|_|", " _|"],
    "+": ["   ", "  |", "  |"],
    "-": ["   ", " _ ", "   "],
    "*": ["   ", " _ ", "   "],
    "/": ["   ", " _ ", "   "],
    "=": ["   ", " _ ", " _ "],
}

# Function to parse input into the seven segment display characters
def parse_input_to_segments(input_lines, n):
    segments = []
    for i in range(n):
        segment = [input_lines[0][i*3:(i+1)*3],
                   input_lines[1][i*3:(i+1)*3],
                   input_lines[2][i*3:(i+1)*3]]
        segments.append(segment)
    return segments

# Function to match a segment with the seven segment map
def segment_to_char(segment):
    for char, representation in seven_segment_map.items():
        if segment == representation:
            return char
    return None

# Function to evaluate the equation
def evaluate_equation(equation):
    parts = equation.split('=')
    lhs = parts[0].strip()
    rhs = parts[1].strip()
    lhs_value = 0
    current_op = '+'
    current_num = ''
    
    for char in lhs:
        if char.isdigit():
            current_num += char
        else:
            if current_num:
                if current_op == '+':
                    lhs_value += int(current_num)
                elif current_op == '-':
                    lhs_value -= int(current_num)
                elif current_op == '*':
                    lhs_value *= int(current_num)
                elif current_op == '/':
                    lhs_value = int(lhs_value / int(current_num))
            current_num = ''
            current_op = char
    
    if current_num:
        if current_op == '+':
            lhs_value += int(current_num)
        elif current_op == '-':
            lhs_value -= int(current_num)
        elif current_op == '*':
            lhs_value *= int(current_num)
        elif current_op == '/':
            lhs_value = int(lhs_value / int(current_num))
    
    return lhs_value == int(rhs)

# Main function to find the faulty segment
def find_faulty_segment(n, input_lines):
    segments = parse_input_to_segments(input_lines, n)
    equation = ''.join(segment_to_char(segment) for segment in segments)
    correct_rhs = equation.split('=')[1]
    
    for i in range(n):
        original_segment = segments[i]
        for char, representation in seven_segment_map.items():
            if segment_to_char(representation) != char:
                segments[i] = representation
                new_equation = ''.join(segment_to_char(segment) for segment in segments)
                if evaluate_equation(new_equation):
                    return i + 1
        segments[i] = original_segment
    
    return -1

# Read input
n = int(input().strip())
input_lines = [input().strip() for _ in range(3)]

# Find the faulty segment
faulty_segment_index = find_faulty_segment(n, input_lines)
print(faulty_segment_index)

#5th problem
def line_intersection(line1, line2):
    # Unpack the line coordinates (x1, y1) to (x2, y2)
    x1, y1, x2, y2 = line1
    x3, y3, x4, y4 = line2

    # Using determinant method to find intersection
    denom = (x1 - x2) * (y3 - y4) - (y1 - y2) * (x3 - x4)
    
    if denom == 0:
        return None  # Lines are parallel or coincident, no intersection
    
    # Calculate intersection point (px, py)
    px = ((x1 * y2 - y1 * x2) * (x3 - x4) - (x1 - x2) * (x3 * y4 - y3 * x4)) / denom
    py = ((x1 * y2 - y1 * x2) * (y3 - y4) - (y1 - y2) * (x3 * y4 - y3 * x4)) / denom
    
    return (px, py)

def calculate_intensity(lines, intersection_point):
    # For simplicity, let's compute the intensity as described
    pass

def main():
    # Input parsing
    N = int(input())  # Number of lines
    lines = []
    for _ in range(N):
        x1, y1, x2, y2 = map(int, input().split())
        lines.append((x1, y1, x2, y2))
    
    K = int(input())  # Type of star we are interested in

    # Find intersections
    intersections = {}  # Dictionary to hold intersections and associated lines
    for i in range(N):
        for j in range(i + 1, N):
            intersection_point = line_intersection(lines[i], lines[j])
            if intersection_point:
                if intersection_point not in intersections:
                    intersections[intersection_point] = set()
                intersections[intersection_point].add(i)
                intersections[intersection_point].add(j)
    
    # Calculate total intensity for stars of type K
    total_intensity = 0
    for intersection_point, lines_set in intersections.items():
        if len(lines_set) == K:
            intensity = calculate_intensity(lines, intersection_point)
            total_intensity += intensity
    
    print(total_intensity)

if __name__ == "__main__":
    main()


#6th problem
def main():
    N = int(input())  # Number of cards
    cards = set()  # Set to store the cards for easy lookup
    
    # Store the cards and track the minimum and maximum bounds
    min_x = min_y = min_z = float('inf')
    max_x = max_y = max_z = float('-inf')
    
    # Parse each card's position and its plane
    for _ in range(N):
        x, y, z, plane = input().split()
        x, y, z = int(x), int(y), int(z)
        
        cards.add((x, y, z, plane))  # Add the card's coordinates and its plane
        
        # Update the min and max bounds
        min_x = min(min_x, x)
        max_x = max(max_x, x)
        min_y = min(min_y, y)
        max_y = max(max_y, y)
        min_z = min(min_z, z)
        max_z = max(max_z, z)
    
    # Check if the box is closed (i.e., it covers a 3D volume)
    # The box should be fully enclosed by cards along each axis.
    
    expected_faces = set()
    
    for x in range(min_x, max_x + 1):
        for y in range(min_y, max_y + 1):
            for z in range(min_z, max_z + 1):
                if (x, y, z, 'xy') not in cards:
                    expected_faces.add((x, y, z, 'xy'))
                if (x, y, z, 'yz') not in cards:
                    expected_faces.add((x, y, z, 'yz'))
                if (x, y, z, 'xz') not in cards:
                    expected_faces.add((x, y, z, 'xz'))
    
    if len(expected_faces) == 0:
        volume = (max_x - min_x + 1) * (max_y - min_y + 1) * (max_z - min_z + 1)
        print(volume)
    else:
        print("Missing cards needed.")  # Adjust this output as needed

if __name__ == "__main__":
    main()


