#include <SFML/Graphics.hpp>
//Другие библиотеки

using namespace sf;

template <typename func>
class Plot
{
private:
	int width = 800, height = 800;
	float scale = 40, step = 0.1, offX = width / 2, offY = height / 2;
	bool onceGrid = true;
	function<func> fun;
public:
	Plot(RenderWindow &window) { window.create(VideoMode(width, height), "Plot"); }
	Plot(int width, int height, float scale, float step, RenderWindow &window) : width(width), height(height), scale(scale), step(step) { window.create(VideoMode(width, height), "Plot"); }
	void grid(RenderWindow &window)
	{
		window.clear(Color::White);
		grid(window, Color::Black);
	}
	void showPlot(function<func> fun, Color col, RenderWindow & window, float xStart = 0, float xEnd = 0)
	{
		if (onceGrid)
		{
			grid(window);
			onceGrid = false;
		}
		VertexArray graf = plot<func>(fun, window, col, xStart, xEnd);
		window.draw(graf);
	}

protected:
	template <typename func>
	VertexArray plot(function<func> fun, RenderWindow &window, Color col, float xS = 0, float xF = 0)
	{
		if (xS == 0 && xF == 0)
		{
			xS = -offX / scale;
			xF = offX / scale;
		}

		VertexArray graf(LinesStrip);
		for (double x = xS; x < xF; x += step)
		{
			double y = fun(x);
			Vertex vertex(Vector2f((x * scale) + offX, (-y * scale) + offY), col);
			graf.append(vertex);
		}

		return graf;
	}

	void grid(RenderWindow &window, Color col)
	{
		VertexArray grid(Lines);
		Vertex line;
		for (int i = offX; i <= width; i += scale)
		{
			for (int j = 0; j < 2; j++)
			{
				line.color = col;
				if (j % 2 == 0)
					line.position = Vector2f(i, 0);
				else
					line.position = Vector2f(i, height);
				grid.append(line);
			}

		}
		for (int i = offX; i >= 0; i -= scale)
		{
			for (int j = 0; j < 2; j++)
			{
				line.color = col;
				if (j % 2 == 0)
					line.position = Vector2f(i, 0);
				else
					line.position = Vector2f(i, height);
				grid.append(line);
			}

		}
		for (int i = offY; i <= height; i += scale)
		{
			for (int j = 0; j < 2; j++)
			{
				line.color = col;
				if (j % 2 == 0)
					line.position = Vector2f(0, i);
				else
					line.position = Vector2f(width, i);
				grid.append(line);
			}

		}
		for (int i = offY; i >= 0; i -= scale)
		{
			for (int j = 0; j < 2; j++)
			{
				line.color = col;
				if (j % 2 == 0)
					line.position = Vector2f(0, i);
				else
					line.position = Vector2f(width, i);
				grid.append(line);
			}

		}
		sf::VertexArray quad(Quads, 8);
		for (int i = 0; i < 8; i++)
			quad[i].color = col;
		quad[0].position = Vector2f(offX - scale / 20, 0);
		quad[1].position = Vector2f(offX + scale / 20, 0);
		quad[2].position = Vector2f(offX + scale / 20, height);
		quad[3].position = Vector2f(offX - scale / 20, height);

		quad[4].position = Vector2f(0, offY + scale / 20);
		quad[5].position = Vector2f(0, offY - scale / 20);
		quad[6].position = Vector2f(width, offY - scale / 20);
		quad[7].position = Vector2f(width, offY + scale / 20);
		window.draw(quad);
		window.draw(grid);
	}
};

int main()
{
	Plot<double(double)> graf(800, 800, 55, 0.05, window);
	bool once = true;
  auto f = [](double x) {return 2 * x - cos(x); };
	while (window.isOpen())
	{
		Event event;

		if (once)
		{
			graf.showPlot(f, Color::Magenta, window);
			window.display();
			once = false;
		}

		window.waitEvent(event);
		if (event.type == Event::Closed)
			window.close();
	}

	return 0;
}
