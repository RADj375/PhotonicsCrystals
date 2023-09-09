# PhotonicsCrystals
Graphics Rendering
C
#include <graphics.h>

void combine_textures(unsigned char *texture1, unsigned char *texture2, unsigned char *output, int width, int height) {
  // Loop through each pixel in the output texture.
  for (int i = 0; i < width * height; i++) {
    // Get the red, green, and blue components of the first texture.
    unsigned char r1 = texture1[i * 3];
    unsigned char g1 = texture1[i * 3 + 1];
    unsigned char b1 = texture1[i * 3 + 2];

    // Get the red, green, and blue components of the second texture.
    unsigned char r2 = texture2[i * 3];
    unsigned char g2 = texture2[i * 3 + 1];
    unsigned char b2 = texture2[i * 3 + 2];

    // Calculate the x and y coordinates of the current pixel in the hexagonal grid.
    int x = i % width;
    int y = i / width;

    // Calculate the weights for the four nearest neighbors.
    float w1 = (1 - x) * (1 - y);
    float w2 = (1 - x) * y;
    float w3 = x * (1 - y);
    float w4 = x * y;

    // Combine the red, green, and blue components of the two textures using hexagonal smooth interpolation.
    unsigned char r = (r1 * w1 + r2 * w2 + r3 * w3 + r4 * w4) / (w1 + w2 + w3 + w4);
    unsigned char g = (g1 * w1 + g2 * w2 + g3 * w3 + g4 * w4) / (w1 + w2 + w3 + w4);
    unsigned char b = (b1 * w1 + b2 * w2 + b3 * w3 + b4 * w4) / (w1 + w2 + w3 + w4);

    // Set the red, green, and blue components of the output texture to the combined values.
    output[i * 3] = r;
    output[i * 3 + 1] = g;
    output[i * 3 + 2] = b;
  }
}

int main() {
  // Initialize graphics mode.
  initgraph(640, 480);

  // Create two textures.
  unsigned char *texture1 = malloc(640 * 480 * 3);
  unsigned char *texture2 = malloc(640 * 480 * 3);

  // Fill the first texture with red.
  for (int i = 0; i < 640 * 480 * 3; i++) {
    texture1[i] = 255;
  }

  // Fill the second texture with green.
  for (int i = 0; i < 640 * 480 * 3; i++) {
    texture2[i] = 0;
  }

  // Create an output texture.
  unsigned char *output = malloc(640 * 480 * 3);

  // Combine the two textures using hexagonal smooth interpolation.
  combine_textures(texture1, texture2, output, 640, 480);

  // Display the output texture.
  putimage(0, 0, output);

  // Wait for the user to press a key.
  getch();

  // Free the memory allocated for the textures.
  free(texture1);
  free(texture2);
  free(output);

  // Close graphics mode.
  closegraph();

  // Create a photonic crystal object.
  struct PhotonicCrystal {
    int lattice_constant;
    float refractive_index;
    int thickness;
  };

  // Create a photonic crystal with a lattice constant of 100 nanometers, a refractive index of 1.5, and a thickness of 100 nanometers.
  struct PhotonicCrystal crystal = {
    .lattice_constant = 100,
    .refractive_index = 1.5,
    .thickness = 100,

