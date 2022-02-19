
![images](https://user-images.githubusercontent.com/93833171/146657547-03b69c02-7ee3-433d-ac65-38e8ad1bbbce.jpg)             ![spread](https://user-images.githubusercontent.com/93833171/146657551-a3ee8a03-f176-4b02-8505-a1dc6eddf4c0.jpg)

* * *
## <span style="color:red">Introduction:</span>
* * *
In our project on QMainWindow, we wrote the code for the chart and the action set for our main SpreadSeet application.and at the end we will create a Text Editor.

## <span style="color:red">Summary:</span>

 * [SpreadSheet (Functionality)](#SpreadSheet)
 

 * [Text Editor](#TextEditor)
     
     adadadaa
     

https://user-images.githubusercontent.com/93819249/150693794-857f07e4-e390-46a9-a12a-8ab0adba1a0a.mp4


#### <span style="color:blue">1-SpreadSheet</span> {#SpreadSheet}

 A spreadsheet is a computer application for organization, analysis, and storage of data in tabular form Or we can say the spreadSheet is worksheet edit a file made of rows and columns that help sort, organize, and arrange data efficiently, and calculate numerical data. What makes a spreadsheet software program unique is its ability to calculate values using mathematical formulas and the data in cells. An example of how a spreadsheet may be utilized is creating an overview of your bank's balance.


<details open>
<summary style="color:black">Used Components:</summary>
<ul>
<li> QPixmap</li>
<li> QMenuBar</li>
<li>QToolBar</li>
<li> QApplication</li>
<li> QFileDialog</li>
<li> QString</li>
    <li>QTextStream</li>
<li> algorithm</li>
<li> QTableWidge</li>
</ul>
</details>

## <span style="color:blue">SpreadSheet.cpp</span>

thats alot but because we working by coding ithout support of designer you will be find some explanation of the code in the commentaire

```cpp
SpreadSheet::SpreadSheet(QWidget *parent)
    : QMainWindow(parent)
{
    //Seting the spreadsheet

    setupMainWidget();

    // Creaeting Actions
    createActions();

    // Creating Menus
    createMenus();


    //Creating the tool bar
    createToolBars();

    //making the connexions
    makeConnexions();


    //Creating the labels for the status bar (should be in its proper function)
    cellLocation = new QLabel("(1, 1)");
    cellFormula = new QLabel("");
    statusBar()->addPermanentWidget(cellLocation);
    statusBar()->addPermanentWidget(cellFormula);

    Currentfile=nullptr;
    setWindowTitle("SpreadSheet");
}

void SpreadSheet::setupMainWidget()
{
    spreadsheet = new QTableWidget;
    spreadsheet->setRowCount(100);
    spreadsheet->setColumnCount(26);
    QStringList labels;
        for(auto letter = 65; letter<=90;letter++)
            labels << QString(letter);
        spreadsheet->setHorizontalHeaderLabels(labels);
    setCentralWidget(spreadsheet);

}

SpreadSheet::~SpreadSheet()
{
    delete spreadsheet;

    // --------------- Actions       --------------//
    delete  newFile;
    delete hello;
    delete  open;
    delete  save;
    delete  saveAs;
    delete  exit;
    delete cut;
    delete copy;
    delete paste;
    delete deleteAction;
    delete find;
    delete row;
    delete Column;
    delete all;
    delete goCell;
    delete recalculate;
    delete sort;
    delete showGrid;
    delete auto_recalculate;
    delete about;
    delete aboutQt;

    // ---------- Menus ----------
    delete FileMenu;
    delete editMenu;
    delete toolsMenu;
    delete optionsMenu;
    delete helpMenu;
}

void SpreadSheet::createActions()
{
    // --------- New File -------------------
   QPixmap newIcon(":/New_icon-.png");
   newFile = new QAction(newIcon, "&New", this);
   newFile->setShortcut(tr("Ctrl+N"));
    // --------- open file -------------------
   QPixmap openIcon(":/open_file.png");
   open = new QAction(openIcon,"&Open", this);
   open->setShortcut(tr("Ctrl+O"));
    // --------- save file -------------------
   QPixmap saveIcon(":/save_file.png");
   save = new QAction(saveIcon,"&Save", this);
   save->setShortcut(tr("Ctrl+S"));
    // --------- SaveAs file -------------------
   QPixmap saveallIcon(":/save_all_file.png");
   saveAs = new QAction(saveallIcon,"save all", this);
   saveAs->setShortcut(tr("Ctrl+shift+S"));
    // --------- cut file -------------------
   QPixmap cutIcon(":/cut.png");
   cut = new QAction(cutIcon, "Cu&t", this);
   cut->setShortcut(tr("Ctrl+X"));
   // --------- copy menu -----------------
   QPixmap copyIcon(":/copy.png");
   copy = new QAction( copyIcon,"&Copy", this);
   copy->setShortcut(tr("Ctrl+C"));
   // paste
    QPixmap pasteIcon(":/paste.png");
    paste = new QAction(pasteIcon,"&Paste", this);
   paste->setShortcut(tr("Ctrl+V"));
   // delete
  QPixmap deleteIcon(":/delete.png");
  deleteAction = new QAction(deleteIcon ,"&Delete", this);
   deleteAction->setShortcut(tr("Del"));
   // --------- Select action -------------------
   QPixmap selectIcon(":/select.png");
   QPixmap selecColumntIcon(":/select_column.png");
   QPixmap selecRowtIcon(":/select_row.png");
   row  = new QAction(selecRowtIcon,"&Row", this);
   Column = new QAction(selecColumntIcon,"&Column", this);
   all = new QAction(selectIcon,"&All", this);
   all->setShortcut(tr("Ctrl+A"));
   // --------- Find mot -------------------
   QPixmap findIcon(":/search_find.png"); find= new QAction(findIcon, "&Find", this);
   find->setShortcut(tr("Ctrl+F"));
   // --------- Go to Cell by the index -------------------
   QPixmap goCellIcon(":/go_to_icon.png");
   goCell = new QAction( goCellIcon, "&Go to Cell", this);
   deleteAction->setShortcut(tr("f5"));
   // --------- Tools -------------------
   recalculate = new QAction("&Recalculate",this);
   recalculate->setShortcut(tr("F9"));
   sort = new QAction("&Sort");
   // --------- Options -------------------
   showGrid = new QAction("&Show Grid");
   showGrid->setCheckable(true);
   showGrid->setChecked(spreadsheet->showGrid());
   auto_recalculate = new QAction("&Auto-recalculate");
   auto_recalculate->setCheckable(true);
   auto_recalculate->setChecked(true);
   // --------- Help -------------------
   about =  new QAction("&About");
   aboutQt = new QAction("About &Qt");
    // --------- exit -------------------
   QPixmap exitIcon(":/Exit_file.png");//for stocke the icon in variable named by exit Icon but the importatn is to knew what QPixmap do
   exit = new QAction(exitIcon,"E&xit", this);
   exit->setShortcut(tr("Ctrl+Q")); //add shortcut
}

void SpreadSheet::close()
{
    auto reply = QMessageBox::warning(this, "Exit",
                                       "Do you really want to quit?",QMessageBox::Yes | QMessageBox::No);//For ask you if you realy want do this

    if(reply == QMessageBox::Yes)
        qApp->exit();
}
void SpreadSheet::createMenus()
{
    // --------  File menu -------//
    FileMenu = menuBar()->addMenu("&File");
    FileMenu->setStyleSheet("background-color: Pink");
    FileMenu->addAction(newFile);
    menuBar()->setStyleSheet("background-color: orange");
    FileMenu->addAction(open);
    FileMenu->addAction(save);
    FileMenu->addAction(saveAs);
    FileMenu->addSeparator();
    FileMenu->addSeparator();
    FileMenu->addAction(exit);
    //------------- Edit menu --------/
    editMenu = menuBar()->addMenu("&Edit");
    editMenu->setStyleSheet("background-color: Magenta");
    editMenu->addAction(cut);
    editMenu->addAction(copy);
    editMenu->addAction(paste);
    editMenu->addAction(deleteAction);
    editMenu->addSeparator();
    //-------------- Select submenu ------------
    auto select = editMenu->addMenu("&Select");
    select->setStyleSheet("background-color: Pink");
    select->addAction(row);
    select->addAction(Column);
    select->addAction(all);
    editMenu->addAction(find);
    editMenu->addAction(goCell);
    //-------------- Toosl menu ------------
    toolsMenu = menuBar()->addMenu("&Tools");
    toolsMenu->setStyleSheet("background-color: 	Teal");
    toolsMenu->addAction(recalculate);
    toolsMenu->addAction(sort);
    //Optins menus
    optionsMenu = menuBar()->addMenu("&Options");
    optionsMenu->setStyleSheet("background-color: Gray");
    optionsMenu->addAction(showGrid);
    optionsMenu->addAction(auto_recalculate);
    //----------- Help menu ------------
    helpMenu = menuBar()->addMenu("&Help");
    helpMenu->setStyleSheet("background-color: Brown");
    helpMenu->addAction(about);
    helpMenu->addAction(aboutQt);
}
void SpreadSheet::createToolBars()
{
    //Crer une bare d'outils
    auto toolbar1 = addToolBar("File");
    //Ajouter des actions acette bar
    toolbar1->addAction(newFile);
    toolbar1->addAction(save);
    toolbar1->addSeparator();//to add line between the component of the  toolbar
    toolbar1->addAction(exit);
    //Creer une autre tool bar
    auto toolbar2  = addToolBar("ToolS");
    toolbar2->addAction(goCell);
}

void SpreadSheet::updateStatusBar(int row, int col)
{
    QString cell{"(%0, %1)"};
   cellLocation->setText(cell.arg(row+1).arg(col+1));
}


void SpreadSheet::makeConnexions()
{ // --------- Connexion for the  select all action ----/
   connect(all, &QAction::triggered,
           spreadsheet, &QTableWidget::selectAll);
   // Connection for the  show grid
   connect(showGrid, &QAction::triggered,spreadsheet, &QTableWidget::setShowGrid);
   //Connection for the exit button
   connect(exit, &QAction::triggered, this, &SpreadSheet::close);
   //Connection for the Help buttons
   connect(about, &QAction::triggered, this, &SpreadSheet::aboutAs);
   connect(aboutQt, &QAction::triggered, this, QApplication::aboutQt);
   //Connection for the select buttons
   connect(row, &QAction::triggered,this, &SpreadSheet::selectrows);
   connect(Column, &QAction::triggered,this, &SpreadSheet::selectcolumns);
   //Connection for the delet buttons
   connect(deleteAction, &QAction::triggered,this, &SpreadSheet::deletes);
//Connection for the FileMenu buttons
   connect(saveAs, &QAction::triggered, this, &SpreadSheet::savesAs);
   connect(save, &QAction::triggered, this, &SpreadSheet::saveslot);
   connect(open, &QAction::triggered, this, &SpreadSheet::openslot);
   //Connection for the find
   connect(find, &QAction::triggered, this, &SpreadSheet::findcell);
   //connectting the chane of any element in the spreadsheet with the update status bar
   connect(spreadsheet, &QTableWidget::cellClicked, this,  &SpreadSheet::updateStatusBar);
   //Connextion between the gocell action and the gocell slot
   connect(goCell, &QAction::triggered, this, &SpreadSheet::goToCellSlot);
}

void SpreadSheet::selectcolumns(){
    if(selected==0){//this for the  first selected in the cell for
        spreadsheet->selectColumn(spreadsheet->currentColumn());//when u chose cell and want to selected everything in the same columns
        selected=spreadsheet->currentColumn();//this for memoires the cell whose selected

    }else{
        spreadsheet->selectColumn(selected);//thats if u in the same selected collumn
selected=0;//this because u cant chose the same columns for more than one time
    }


}void SpreadSheet::selectrows(){
    if(selected==0){//this for the  first selected in the cell for
        spreadsheet->selectRow(spreadsheet->currentRow());//when u chose cell and want to selected everything in the same rows
        selected=spreadsheet->currentRow();//this for stocke the cell whose selected

    }else{
        spreadsheet->selectRow(selected);//thats if u in the same selected rows
        selected=0;//this because u cant chose the same rows for more than one time
    };
}
void SpreadSheet::deletes(){//for delet the selected cell by
    for( QTableWidgetItem *item : spreadsheet->selectedItems())//for each item in the cells u selected if find item or writting  deleted
        delete item;//for delet the item after getting by  the boucle
}
void SpreadSheet::aboutAs(){
    QMessageBox::information(this, "About", "Hello we are said and adil if u have any information about this project u can contact as :");//for information of help
}
void SpreadSheet::goToCellSlot()
 {
    //Creating the dialog
    GoDialog D;
     //Executing the dialog and storing the user response
     auto repl = D.exec();
    //Checking if the dialog is accepted
     if(repl == GoDialog::Accepted)
     {
        auto text = D.getText();//because we use the designer we need do thats by this way for get the data of the cell who we are searching for him
        int row = text[0].toLatin1() - 'A';//thats because we have in information entred in the other interface of designer to type of date character=row and number=col
       text = text.remove(0,1);//for remove the number of row because we get it before
       int col = text.toInt()-1;//because wa start from 0 not 1
       spreadsheet->setCurrentCell(col,row);//this command for selected the cell of rows,col

 }
}

void SpreadSheet::findcell()
 {
    finddialog f;
     auto reply = f.exec();
     if(reply == finddialog::Accepted)
     {

         auto cell = f.getText();
//all this thing for check every cell the the spread sheet for the word whose searching for himssd
   for(int i=0;i<spreadsheet->rowCount();i++)
   {
       for(int j=0;j<spreadsheet->columnCount();j++)
       {
           if(spreadsheet->item(i,j))
           {
                if(spreadsheet->item(i,j)->text()==cell)//test if the same word
                      spreadsheet->setCurrentCell( i,  j);
           }
       }
   }
 }
}
void SpreadSheet::saveslot(){
    if(!Currentfile){//thats contition just because we need the interface of save open just one time when the file not exist or new file and after save automatuicley
        QFileDialog D;
        auto filename=D.getSaveFileName(this,"Save Spreadsheet", ".","Spreadsheet files (*.sp)");//in the interface of saving the file the first is the name of interface the second is the caption for the file and the last is the type of direction
        Currentfile=new QString(filename);
        setWindowTitle(QFileInfo(filename).fileName());//thats for show just the specific name without the path
    }
    savecontent(*Currentfile);//the main function of saving
}
void SpreadSheet::savesAs(){//the same things with save but this always show the interface of saving for change the name or save in another directory
    QFileDialog D;
    auto filename=D.getSaveFileName(this,"Save Spreadsheet", ".","Spreadsheet files (*.sp)");

    Currentfile=new QString(filename);
    setWindowTitle(QFileInfo(filename).fileName());
    //QMessageBox::information(this,"Tile","you chose ur file name");
    savecontent(*Currentfile);
}
void SpreadSheet::openslot(){
//the same works of save but the opposit : because tthis open the file
        QFileDialog D;
        auto filename=D.getOpenFileName(this,"Open Spreadsheet", ".","Spreadsheet files (*.sp)");//here the dir is sp(spreadSheet)
        Currentfile=new QString(filename);
         //QFileInfo(filename).fileName();
        setWindowTitle(QFileInfo(filename).fileName());
    if(i<5){//condition for show just the five recent file open  he count the same file if u open it again
        hello = new QAction(QFileInfo(filename).fileName(),this);//to ccreate new action in the filemenu to see the recent file after open  file
        hello->setShortcut(tr("F9"));
        FileMenu->addAction(QFileInfo(filename).fileName());
        openRecent();//the main finction of recent file
        i++;
}
    loadcontent(*Currentfile);
}
void SpreadSheet::savecontent(QString filename){

    QFile file( filename);
    if(file.open(QIODevice::WriteOnly)){
        QTextStream out(&file);
        for (int i=0;i<spreadsheet->rowCount() ;i++ ) {
            for(int j=0; j<spreadsheet->columnCount();j++){
                auto cell=spreadsheet->item(i,j);
                if(cell){
                    out<<i<<","<<j<<","<<cell->text()<<endl;
                }
            }

        }
}
}
void SpreadSheet::openRecent(){//the main funtion of recent file
    QAction *action = qobject_cast<QAction *>(sender());//for get the last signal of action to add the file who open in the filemenu
    if (action)
    {
        loadcontent(action->text());
    }
}
void SpreadSheet::loadcontent(QString filename){//the main function of open file
    spreadsheet->clear();//this for open the fie in new Spreadsheet
   QFile file(filename);
    if(file.open(QIODevice::ReadOnly)){//for reading the file
        QTextStream in(&file);//stocke the file in in

        while(!in.atEnd()){//boucle for get ivery word in the file and add the ',' to parameter of word ro
           QString line=in.readLine();
           auto tokens=line.split(QChar(','));
           int row=tokens[0].toInt();
           int col=tokens[1].toInt();
           auto cell=new QTableWidgetItem(tokens[2]);
           spreadsheet->setItem(row,col,cell);
           //spreadsheet.setfor
        }
}
    file.close();
}

```

## <span style="color:blue">SpreadSheet.h</span>


```cpp

class SpreadSheet : public QMainWindow
{
    Q_OBJECT

public:
    SpreadSheet(QWidget *parent = nullptr);
    ~SpreadSheet();
    int selected=0;
    int selects=0;
protected:
    void setupMainWidget();
    void createActions();
    void createMenus();
    void createToolBars();
    void makeConnexions();
private slots:
    void close();
    void updateStatusBar(int, int); //Respond for the call changed
    void goToCellSlot();
    void findcell();
    void saveslot();
    void openslot();
    void aboutAs();
    void selectrows();
    void selectcolumns();
    void deletes();
    void savesAs();
    //for affiche les recent file
   void openRecent();
 //Pointers
private:
    void savecontent(QString filename);
    void loadcontent(QString filename);
    // --------------- Central Widget -------------//
    QTableWidget *spreadsheet;
    // --------------- Actions       --------------//
    QAction * newFile;
    QAction * open;
    QAction * save;
    QAction * saveAs;
    QAction * exit;
    QAction *cut;
    QAction *copy;
    QAction *paste;
    QAction *deleteAction;
    QAction *find;
    QAction *row;
    QAction *Column;
    QAction *all;
    QAction *goCell;
    QAction *recalculate;
    QAction *sort;
    QAction *showGrid;
    QAction *auto_recalculate;
    QAction *about;
    QAction *aboutQt;
    //for recent file action
    QAction *hello;
    // ---------- Menus ----------
    QMenu *FileMenu;
    QMenu *editMenu;
    QMenu *toolsMenu;
    QMenu *optionsMenu;
    QMenu *helpMenu;
    QString *Currentfile=nullptr;
 int i=0;
    //  ----- - Widget pouyr la bare d'etat
    QLabel *cellLocation;  //position de la cellule active
    QLabel *cellFormula;   // Formuel de la cellue active
};

```

## <span style="color:blue">main.cpp</span>


```cpp

int main(int argc, char *argv[])
{
    QApplication a(argc, argv);
    SpreadSheet w;
    w.show();
    return a.exec();
}

```
## <span style="color:blue">finddialog.ui</span>


![2021-12-19 05_01_38-SpreadSheet](https://user-images.githubusercontent.com/93819249/146663446-642d0556-ff66-4ffb-8e9c-be8d0106c079.png)


## <span style="color:blue">gocell.ui</span>


![2021-12-19 05_01_18-SpreadSheet](https://user-images.githubusercontent.com/93819249/146663447-bc4d6ccc-1f42-403f-a8ff-f9aab730447d.png)


## <span style="color:blue">The Result</span>

![2021-12-19 05_14_12-](https://user-images.githubusercontent.com/93819249/146663615-d69cc6c0-2a82-4893-a126-c990033fc411.png)

![WhatsApp Image 2021-12-19 at 01 55 13](https://user-images.githubusercontent.com/93819249/146663619-177be8e1-73ee-4654-abfd-d5177cbfda23.jpeg)






## <span style="color:blue">2-Text Editor</span> {#TextEditor}

* * *
## <span style="color:blue">main.cpp</span>
```cpp
#include "mainwindow.h"

#include <QApplication>

int main(int argc, char *argv[])
{
    QApplication a(argc, argv);
    MainWindow w;
    w.show();
    return a.exec();
}

```


* * *
## <span style="color:orange">mainwindow.cpp</span>
```cpp

#include "mainwindow.h"
#include "ui_mainwindow.h"
#include<QFile>
#include<QFileDialog>
#include<QTextStream>
#include<QMessageBox>
MainWindow::MainWindow(QWidget *parent)
    : QMainWindow(parent)
    , ui(new Ui::MainWindow)
{
    ui->setupUi(this);
    this->setCentralWidget(ui->textEdit);
}

MainWindow::~MainWindow()
{
    delete ui;
}

//new function
void MainWindow::on_actionNew_triggered()
{
   file_path="";
   ui->textEdit->setText("");
}

//copy function
void MainWindow::on_actionCopy_triggered()
{
    ui->textEdit->copy();
}

//paste function
void MainWindow::on_actionPaste_triggered()
{
     ui->textEdit->paste();
}

//cut function
void MainWindow::on_actionCut_triggered()
{
   ui->textEdit->cut();
}

//open function
void MainWindow::on_actionOpen_triggered()
{
    QString file_name=QFileDialog::getOpenFileName(this,"ouvrir le fichier ");
    QFile file(file_name);
    file_path=file_name;
    if(!file.open(QFile::ReadOnly | QFile::Text)){
      QMessageBox::warning(this,"...","file not open");
      return;
    }

    QTextStream in(&file);
    QString text=in.readAll();
    ui->textEdit->setText(text);
    file.close();
}

// save function
void MainWindow::on_actionSave_triggered()
{
    //QString file_name=QFileDialog::getSaveFileName(this,"enregistrer le fichier ");
    QFile file(file_path);
    if(!file.open(QFile::WriteOnly | QFile::Text)){
      QMessageBox::warning(this,"...","file not open");
      return;
    }

    QTextStream out(&file);
    QString text=ui->textEdit->toPlainText();
    out<<text;
    file.flush();
    file.close();
}

//save_As function
void MainWindow::on_actionSave_As_triggered()
{
    QString file_name=QFileDialog::getSaveFileName(this,"enregistrer le fichier ");
    QFile file(file_name);
    file_path=file_name;
    if(!file.open(QFile::WriteOnly | QFile::Text)){
      QMessageBox::warning(this,"...","file not open");
      return;
    }

    QTextStream out(&file);
    QString text=ui->textEdit->toPlainText();
    out<<text;
    file.flush();
    file.close();
}

//about function
void MainWindow::on_actionAbout_triggered()
{
    QString about_qt;
    about_qt="I'm SAID ET ADIL  , you're welcome";
    about_qt+="date: 19/12/2021";

    QMessageBox::about(this,"qt",about_qt);
}

//about_qt function
void MainWindow::on_actionAbout_qt_triggered()
{
    QString about_qt;

    QMessageBox::aboutQt(this,about_qt);
}

// exit function
void MainWindow::on_actionExit_triggered()
{
    QApplication::quit();
}


```
* *  *

#### <span style="color:orange">mainwindow.h</span>

* * *
```h
#ifndef MAINWINDOW_H
#define MAINWINDOW_H

#include <QMainWindow>

QT_BEGIN_NAMESPACE
namespace Ui { class MainWindow; }
QT_END_NAMESPACE

class MainWindow : public QMainWindow
{
    Q_OBJECT

public:
    MainWindow(QWidget *parent = nullptr);
    ~MainWindow();

private slots:
    void on_actionNew_triggered();

    void on_actionCopy_triggered();

    void on_actionPaste_triggered();

    void on_actionCut_triggered();

    void on_actionOpen_triggered();

    void on_actionSave_triggered();

    void on_actionSave_As_triggered();

    void on_actionAbout_triggered();

    void on_actionAbout_qt_triggered();

    void on_actionExit_triggered();

private:
    Ui::MainWindow *ui;
    QString file_path;
};
#endif // MAINWINDOW_H
```
* * *
## <span style="color:orange">mainwindow.ui</span>
* * *

![qt_dessiner](https://user-images.githubusercontent.com/93833171/146673512-b48d7424-cef6-49fc-8d1b-2fb54edc7856.jpg)

at the end we find our application like this:

![qt_dessin](https://user-images.githubusercontent.com/93833171/146659946-569c30ae-5465-479b-9983-0989a9a7efc0.jpg)

Here is an overview of the menus:

![WhatsApp Image 2021-12-19 at 02 00 57](https://user-images.githubusercontent.com/93833171/146659963-2c3ce7c4-ca3e-4783-af47-cfd6db55b71b.jpeg)

## <span style="color:red">Conclusion:</span>
In our project, we presented the essence of an application in QT. We have our how to create the spreadsheet application in details, and at the end we showed how to use QT Desinger to create a Text Editor.

