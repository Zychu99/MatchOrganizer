#include <windows.h>

LPSTR NazwaKlasy = "Klasa Okienka";
MSG Komunikat;

LRESULT CALLBACK WndProc( HWND hwnd, UINT msg, WPARAM wParam, LPARAM lParam );

int WINAPI WinMain( HINSTANCE hInstance, HINSTANCE hPrevInstance, LPSTR lpCmdLine, int nCmdShow )
{
   
    // WYPEŁNIANIE STRUKTURY
    WNDCLASSEX wc;
   
    wc.cbSize = sizeof( WNDCLASSEX );
    wc.style = 0;
    wc.lpfnWndProc = WndProc;
    wc.cbClsExtra = 0;
    wc.cbWndExtra = 0;
    wc.hInstance = hInstance;
    wc.hIcon = LoadIcon( NULL, IDI_APPLICATION );
    wc.hCursor = LoadCursor( NULL, IDC_ARROW );
    wc.hbrBackground =( HBRUSH )( COLOR_WINDOW + 1 );
    wc.lpszMenuName = NULL;
    wc.lpszClassName = NazwaKlasy;
    wc.hIconSm = LoadIcon( NULL, IDI_APPLICATION );
   
    // REJESTROWANIE KLASY OKNA
    if( !RegisterClassEx( & wc ) )
    {
        MessageBox( NULL, "Wysoka Komisja odmawia rejestracji tego okna!", "Niestety...",
        MB_ICONEXCLAMATION | MB_OK );
        return 1;
    }
   
    // TWORZENIE OKNA
    HWND hwnd;
   
    hwnd = CreateWindowEx( WS_EX_CLIENTEDGE, NazwaKlasy, "Match-Organizer", WS_OVERLAPPEDWINDOW,
    CW_USEDEFAULT, CW_USEDEFAULT, 240, 120, NULL, NULL, hInstance, NULL );
   
    if( hwnd == NULL )
    {
        MessageBox( NULL, "Okno odmówiło przyjścia na świat!", "Ale kicha...", MB_ICONEXCLAMATION );
        return 1;
    }
   
    ShowWindow( hwnd, nCmdShow ); // Pokaż okienko...
    UpdateWindow( hwnd );
    
    // TWORZENIE PRZYCISKU 1
    HWND g_hPrzycisk1;
    
    g_hPrzycisk1 = CreateWindowEx( 0, "BUTTON", "DRUŻYNY", WS_CHILD | WS_VISIBLE,
100, 100, 150, 30, hwnd, NULL, hInstance, NULL );

   // TWORZENIE PRZYCISKU 2
    HWND g_hPrzycisk2;
 
    g_hPrzycisk2 = CreateWindowEx( 0, "BUTTON", "DRUŻYNA A", WS_CHILD | WS_VISIBLE | BS_CHECKBOX,
100, 130, 150, 30, hwnd, NULL, hInstance, NULL );

     // TWORZENIE PRZYCISKU 3
    HWND g_hPrzycisk3;
 
    g_hPrzycisk3 = CreateWindowEx( 0, "BUTTON", "DRUŻYNA B", WS_CHILD | WS_VISIBLE | BS_CHECKBOX,
100, 160, 150, 30, hwnd, NULL, hInstance, NULL );
     // POLE TEKSTOWE 
HWND hText = CreateWindowEx( WS_EX_CLIENTEDGE, "EDIT", NULL, WS_CHILD | WS_VISIBLE | WS_BORDER |
WS_VSCROLL | ES_MULTILINE | ES_AUTOVSCROLL, 100, 200, 150, 30, hwnd, NULL, hInstance, NULL );
     
    // Pętla komunikatów
    while( GetMessage( & Komunikat, NULL, 0, 0 ) )
    {
        TranslateMessage( & Komunikat );
        DispatchMessage( & Komunikat );
    }
    return Komunikat.wParam;
}

// OBSŁUGA ZDARZEŃ
LRESULT CALLBACK WndProc( HWND hwnd, UINT msg, WPARAM wParam, LPARAM lParam )
{
    switch( msg )
    {
    case WM_CLOSE:
        DestroyWindow( hwnd );
        break;
       
    case WM_DESTROY:
        PostQuitMessage( 0 );
        break;
       
        default:
        return DefWindowProc( hwnd, msg, wParam, lParam );
    }
   
    return 0;
}
