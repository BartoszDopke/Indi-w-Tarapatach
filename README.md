# Indi-w-Tarapatach
W repozytorium umieszczam kod źródłowy, specyfikację i inne pliki potrzebne do stworzenia gry "Indi w Tarapatach!" Wszelkie prawa do kopiowania zastrzeżone.


SOURCE CODE:
#include <stdio.h>
#include <allegro5/allegro.h>
#include <allegro5/allegro_image.h>
#include <allegro5/allegro_font.h>
#include <allegro5/allegro_ttf.h>

#define BACKGROUND_FILE     "mapa.png"
const float FPS = 60;
const int SCREEN_W = 1280;
const int SCREEN_H = 720;
const int WARRIOR_SIZE = 84;
enum MYKEYS {
	KEY_UP, KEY_DOWN, KEY_LEFT, KEY_RIGHT
};

int main(int argc, char **argv)
{
	ALLEGRO_DISPLAY *display = NULL;
	/*ALLEGRO_DISPLAY *displaystart = NULL; */
	ALLEGRO_EVENT_QUEUE *event_queue = NULL;
	ALLEGRO_TIMER *timer = NULL;
	float warrior_x = SCREEN_W / 2.0 - WARRIOR_SIZE / 2.0;
	float warrior_y = SCREEN_H / 2.0 - WARRIOR_SIZE / 2.0;
	bool key[4] = { false, false, false, false };
	bool redraw = true;
	bool doexit = false;

	if (!al_init()) {
		fprintf(stderr, "failed to initialize allegro!\n");
		return -1;
	}
	/*al_init_font_addon(); // initialize the font addon
	al_init_ttf_addon();// initialize the ttf (True Type Font) addon

	displaystart = al_create_display(1024, 768);
	if (!displaystart) {
		fprintf(stderr, "failed to create displaystart!\n");
		return -1;
	}
	ALLEGRO_FONT *font = al_load_ttf_font("Artesana.ttf", 64, 0);
	ALLEGRO_FONT *fonttytul = al_load_ttf_font("ARRIBAAR.TTF", 80, 0);

	if (!font) {
		fprintf(stderr, "Could not load 'Artesana.ttf'.\n");
		return -1;
	}
	if (!fonttytul) {
		fprintf(stderr, "Could not load 'ARRIBAAR.TTF'.\n");
		return -1;
	}

	al_clear_to_color(al_map_rgb(2, 147, 0));
	al_draw_text(fonttytul, al_map_rgb(223, 234, 18), 1024 / 2, 80, ALLEGRO_ALIGN_CENTRE, "Indi w tarapatach!");
	al_draw_text(font, al_map_rgb(255, 255, 255), 1024 / 2, 250 , ALLEGRO_ALIGN_CENTRE, "Nowa gra");
	al_draw_text(font, al_map_rgb(255, 255, 255), 1024 / 2, 400, ALLEGRO_ALIGN_CENTRE, "Wyjdz z gry");
	al_flip_display();

	al_rest(5.0);

	al_destroy_display(displaystart);

	return 0;
} */
	 if (!al_install_keyboard()) {
		fprintf(stderr, "failed to initialize the keyboard!\n");
		return -1;
	}
	
	timer = al_create_timer(0.8 / FPS);
	if (!timer) {
		fprintf(stderr, "failed to create timer!\n");
		return -1;
	}
	if (!al_init_image_addon()) {
		fprintf(stderr, "Failed to initialize al_init_image_addon!\n");
		return -1;
	}
	display = al_create_display(SCREEN_W, SCREEN_H); 
	if (!display) {
		fprintf(stderr, "failed to create display!\n");
		al_destroy_timer(timer);
		return -1;
	}
	ALLEGRO_BITMAP	*warrior = al_load_bitmap("indiana.png");
	ALLEGRO_BITMAP  *background = NULL;
	warrior = al_load_bitmap("indiana.png");
	if (!warrior) {
		fprintf(stderr, "failed to create warrior!\n");
		al_destroy_display(display);
		al_destroy_timer(timer);
		return 0;
	}
	background = al_load_bitmap(BACKGROUND_FILE);
	if (!background)
	{
		fprintf(stderr, "failed to load background bitmap!\n");
		return -1;
	}

	al_set_target_bitmap(warrior);

	al_set_target_bitmap(al_get_backbuffer(display));

	event_queue = al_create_event_queue();
	if (!event_queue) {
		fprintf(stderr, "failed to create event_queue!\n");
		al_destroy_bitmap(warrior);
		al_destroy_display(display);
		al_destroy_timer(timer);
		return -1;
	}

	al_register_event_source(event_queue, al_get_display_event_source(display));

	al_register_event_source(event_queue, al_get_timer_event_source(timer));

	al_register_event_source(event_queue, al_get_keyboard_event_source());

	al_draw_bitmap(background, 0, 0, 0);

	al_flip_display();

	al_start_timer(timer);

	while (!doexit)
	{
		ALLEGRO_EVENT ev;
		al_wait_for_event(event_queue, &ev);

		if (ev.type == ALLEGRO_EVENT_TIMER) {
			if (key[KEY_UP] && warrior_y >= 4.0) {
				warrior_y -= 4.0;
			}

			if (key[KEY_DOWN] && warrior_y <= SCREEN_H - WARRIOR_SIZE - 4.0) {
				warrior_y += 4.0;
			}

			if (key[KEY_LEFT] && warrior_x >= 4.0) {
				warrior_x -= 4.0;
			}

			if (key[KEY_RIGHT] && warrior_x <= SCREEN_W - WARRIOR_SIZE - 4.0) {
				warrior_x += 4.0;
			}
			redraw = true;
		}
		else if (ev.type == ALLEGRO_EVENT_DISPLAY_CLOSE) {
			break;
		}
		else if (ev.type == ALLEGRO_EVENT_KEY_DOWN) {
			switch (ev.keyboard.keycode) {
			case ALLEGRO_KEY_UP:
				key[KEY_UP] = true;
				break;

			case ALLEGRO_KEY_DOWN:
				key[KEY_DOWN] = true;
				break;

			case ALLEGRO_KEY_LEFT:
				key[KEY_LEFT] = true;
				break;

			case ALLEGRO_KEY_RIGHT:
				key[KEY_RIGHT] = true;
				break;
			}
		}
		else if (ev.type == ALLEGRO_EVENT_KEY_UP) {
			switch (ev.keyboard.keycode) {
			case ALLEGRO_KEY_UP:
				key[KEY_UP] = false;
				break;

			case ALLEGRO_KEY_DOWN:
				key[KEY_DOWN] = false;
				break;

			case ALLEGRO_KEY_LEFT:
				key[KEY_LEFT] = false;
				break;

			case ALLEGRO_KEY_RIGHT:
				key[KEY_RIGHT] = false;
				break;

			case ALLEGRO_KEY_ESCAPE:
				doexit = true;
				break;
			}
		}

		if (redraw && al_is_event_queue_empty(event_queue)) {
			redraw = false;


			al_draw_bitmap(warrior, warrior_x, warrior_y, 0);
			al_flip_display();
		}
	} 
	al_destroy_bitmap(warrior);
	al_destroy_bitmap(background);
	al_destroy_timer(timer);
	al_destroy_display(display);
	al_destroy_event_queue(event_queue);

	return 0;
} 
