#include "views/gamelist/IGameListView.h"
#include "Window.h"
#include "guis/GuiMetaDataEd.h"
#include "guis/GuiMenu.h"
#include "guis/GuiGamelistOptions.h"
#include "views/ViewController.h"
#include "Settings.h"
#include "Log.h"
#include "Sound.h"

#include <stdio.h> //libreria para el archivo

static const std::string LETTERS = "#ABCDEFGHIJKLMNOPQRSTUVWXYZ"; //agregada

bool IGameListView::input(InputConfig* config, Input input)
{


//-------------------------------------Modificaciones propias-----------------------------------------------
	//Las siguientes 2 IF son agregados
	if(config->isMappedTo("pageup", input) && input.value)
	{
		char letraActual,letraSgte;
		int letraActualId, letraSgteId;
			
		//SDL_ShowSimpleMessageBox(SDL_MESSAGEBOX_ERROR, "TITULO","MENSAJE", NULL);
		SystemData* mSystem = this->mRoot->getSystem();
		IGameListView* gamelist = ViewController::get()->getGameListView(mSystem).get();
		letraActual = toupper(gamelist->getCursor()->getName()[0]);
		
		//Identifico si es numero o letra
		if (letraActual >= '0' && letraActual <= '9')
		{
			letraActualId = 0; //Posicion 0 dentro del array abcedario
		}else
		{
			letraActualId = LETTERS.find(letraActual); //Posicion dentro del array abcedario
		}

		letraSgteId = letraActualId-1;

		
		const std::vector<FileData*>& list = gamelist->getCursor()->getParent()->getChildren();

		bool exito = false;
		while(!exito)
		{
			// find the first entry in the list that either exactly matches our target letter or is beyond our target letter
			for(auto it = list.cbegin(); it != list.cend(); it++)
			{
				char check = (*it)->getName().empty() ? 'A' : (*it)->getName()[0];

				// if there's an exact match or we've passed it, set the cursor to this one
				if(check == LETTERS[letraSgteId])
				{
					gamelist->setCursor(*it);
					exito = true;
					break;
				}
			}
			letraSgteId--;
		}
	}
	

	if(config->isMappedTo("pagedown", input) && input.value)
	{
		char letraActual,letraSgte;
		int letraActualId, letraSgteId;
			
		//SDL_ShowSimpleMessageBox(SDL_MESSAGEBOX_ERROR, "TITULO","MENSAJE", NULL);
		SystemData* mSystem = this->mRoot->getSystem();
		IGameListView* gamelist = ViewController::get()->getGameListView(mSystem).get();
		letraActual = toupper(gamelist->getCursor()->getName()[0]);
		
		//Identifico si es numero o letra
		if (letraActual >= '0' && letraActual <= '9')
		{
			letraActualId = 0; //Posicion 0 dentro del array abcedario
		}else
		{
			letraActualId = LETTERS.find(letraActual); //Posicion dentro del array abcedario
		}

		letraSgteId = letraActualId+1;

		
		const std::vector<FileData*>& list = gamelist->getCursor()->getParent()->getChildren();

		bool exito = false;
		while(!exito)
		{
			// find the first entry in the list that either exactly matches our target letter or is beyond our target letter
			for(auto it = list.cbegin(); it != list.cend(); it++)
			{
				char check = (*it)->getName().empty() ? 'A' : (*it)->getName()[0];

				// if there's an exact match or we've passed it, set the cursor to this one
				if(check == LETTERS[letraSgteId])
				{
					gamelist->setCursor(*it);
					exito = true;
					break;
				}
			}
			letraSgteId++;
		}
  		//Fichero para guardar resultados
  		/*FILE* pFile = fopen("logFile.txt", "a");
  		fprintf(pFile, "%c\n",LETTERS[letraSgteId]);
  		fclose(pFile);*/
 
	}

//------------------------------------------------------------------------------------------------------------

	// select to open GuiGamelistOptions
	if(config->isMappedTo("select", input) && input.value)
	{
		Sound::getFromTheme(mTheme, getName(), "menuOpen")->play();
		mWindow->pushGui(new GuiGamelistOptions(mWindow, this->mRoot->getSystem()));
		return true;

	// Ctrl-R to reload a view when debugging
	}else if(Settings::getInstance()->getBool("Debug") && config->getDeviceId() == DEVICE_KEYBOARD && 
		(SDL_GetModState() & (KMOD_LCTRL | KMOD_RCTRL)) && input.id == SDLK_r && input.value != 0)
	{
		LOG(LogDebug) << "reloading view";
		ViewController::get()->reloadGameListView(this, true);
		return true;
	}

	return GuiComponent::input(config, input);
}

void IGameListView::setTheme(const std::shared_ptr<ThemeData>& theme)
{
	mTheme = theme;
	onThemeChanged(theme);
}

HelpStyle IGameListView::getHelpStyle()
{
	HelpStyle style;
	style.applyTheme(mTheme, getName());
	return style;
}
