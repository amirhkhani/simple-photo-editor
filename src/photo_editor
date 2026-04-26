from PyQt6.QtWidgets import QApplication, QMainWindow, QLabel, QMessageBox, QWidget, QStatusBar, QFileDialog, QToolBar, \
    QDockWidget, QPushButton, QVBoxLayout
from PyQt6.QtCore import Qt, QSize
from PyQt6.QtGui import QPixmap, QAction, QIcon, QTransform
from functools import partial
from sys import exit


class PhotoEditor(QMainWindow):
    def __init__(self):
        super().__init__()
        self.image = QPixmap()
        self.last_image = QPixmap()

        self.config_main_window()

        self.add_menubar()

        self.open_action()
        self.save_action()
        self.exit_action()

        self.rotate90_action()
        self.rotate180_action()
        self.flip_horizontal_action()
        self.flip_vertical_action()
        self.resize_half_action()
        self.reset_image_action()
        self.set_full_screen_action()
        self.clear_image_action()
        self.dock_widget_buttons()
        self.dock_widget_toggled_action()
        self.add_dock_widget_buttons()

        self.show()

    def config_main_window(self):
        self.setWindowTitle("Photo Editor GUI")
        self.setFixedSize(QSize(1000, 800))
        self.move((1920 // 2) - (self.width() // 2),
                  self.height() // (self.height() // 100))

        self.setStatusBar(QStatusBar())
        self.statusBar().setStyleSheet("font-size:18px;")

        self.central_label = QLabel(self)
        self.setCentralWidget(self.central_label)

        self.toolbar = QToolBar("ToolBar")
        self.addToolBar(self.toolbar)
        self.toolbar.setIconSize(QSize(40, 40))

        self.dock_widget = QDockWidget("DockWidget")
        self.dock_widget.setWindowTitle("Tools Window")
        self.dock_widget.setStyleSheet("font-size:18px;")
        self.dock_widget.setAllowedAreas(Qt.DockWidgetArea.RightDockWidgetArea)


    def add_menubar(self):
        menu = self.menuBar()
        menu.setStyleSheet("font-size: 20px")

        self.file_menu = menu.addMenu("&File")
        self.edit_menu = menu.addMenu("&Edit")
        self.view_menu = menu.addMenu("&View")

    def open_action(self):
        self.open_act = QAction(QIcon("../images/open.png"), "Open")
        self.open_act.setShortcut("Ctrl+O")
        self.open_act.setStatusTip("Open new file for editing")
        self.open_act.triggered.connect(self.open_photo)

        self.file_menu.addAction(self.open_act)
        self.toolbar.addAction(self.open_act)

    def save_action(self):
        self.save_act = QAction(QIcon("../images/save.png"), "Save")
        self.save_act.setShortcut("Ctrl+S")
        self.save_act.setStatusTip("Save the edited photo")
        self.save_act.triggered.connect(self.save_photo)

        self.file_menu.addAction(self.save_act)
        self.toolbar.addAction(self.save_act)
        self.file_menu.addSeparator()

    def exit_action(self):
        self.exit_act = QAction(QIcon("../images/quit.png"), "Exit")
        self.exit_act.setShortcut("Ctrl+Q")
        self.exit_act.setStatusTip("Exit application")
        self.exit_act.triggered.connect(self.close)

        self.file_menu.addAction(self.exit_act)

    def rotate90_action(self):
        self.rotate90_act = QAction("Rotate 90")
        self.rotate90_act.setShortcut("Ctrl+9")
        self.rotate90_act.setStatusTip("The photo will be Rotated 90 degrees")
        self.rotate90_act.triggered.connect(partial(self.rotate, 90))

        self.edit_menu.addAction(self.rotate90_act)

    def rotate180_action(self):
        self.rotate180_act = QAction("Rotate 180")
        self.rotate180_act.setShortcut("Ctrl+1")
        self.rotate180_act.setStatusTip("The photo will be Rotated 180 degrees")
        self.rotate180_act.triggered.connect(partial(self.rotate, 180))

        self.edit_menu.addAction(self.rotate180_act)
        self.edit_menu.addSeparator()

    def set_full_screen_action(self):
        self.full_screen_act = QAction("Full screen")
        self.full_screen_act.setShortcut("Ctrl+F")
        self.full_screen_act.setStatusTip("The photo will be scaled to full screen")
        self.full_screen_act.triggered.connect(self.set_full_screen)

        self.edit_menu.addAction(self.full_screen_act)
        self.edit_menu.addSeparator()

    def flip_horizontal_action(self):
        self.flip_horizontal_act = QAction("Flip Horizontal")
        self.flip_horizontal_act.setShortcut("Ctrl+H")
        self.flip_horizontal_act.setStatusTip("The photo will be flipped horizontally")
        self.flip_horizontal_act.triggered.connect(partial(self.flip, -1, 1))

        self.edit_menu.addAction(self.flip_horizontal_act)

    def flip_vertical_action(self):
        self.flip_vertical_act = QAction("Flip Vertical")
        self.flip_vertical_act.setShortcut("Ctrl+V")
        self.flip_vertical_act.setStatusTip("The photo will be flipped vertically")
        self.flip_vertical_act.triggered.connect(partial(self.flip, 1, -1))

        self.edit_menu.addAction(self.flip_vertical_act)
        self.edit_menu.addSeparator()

    def resize_half_action(self):
        self.resize_half_act = QAction("Resize Half")
        self.resize_half_act.setShortcut("Ctrl+L")
        self.resize_half_act.setStatusTip("Will be scale photo 0.5 by 0.5")
        self.resize_half_act.triggered.connect(partial(self.flip, 0.5, 0.5))

        self.edit_menu.addAction(self.resize_half_act)
        self.edit_menu.addSeparator()

    def reset_image_action(self):
        self.reset_image_act = QAction(QIcon("../images/redo.png"), "Reset Image")
        self.reset_image_act.setShortcut("Ctrl+R")
        self.reset_image_act.setStatusTip("Will be reset image to last saved")
        self.reset_image_act.triggered.connect(self.reset_photo)

        self.edit_menu.addAction(self.reset_image_act)
        self.toolbar.addAction(self.reset_image_act)

    def clear_image_action(self):
        self.clear_image_act = QAction(QIcon("../images/delete.png"), "Clear Image")
        self.clear_image_act.setShortcut("Ctrl+D")
        self.clear_image_act.setStatusTip("Will be clear the photo")
        self.clear_image_act.triggered.connect(self.clear_image)

        self.edit_menu.addAction(self.clear_image_act)
        self.toolbar.addAction(self.reset_image_act)
        self.toolbar.addAction(self.exit_act)


    def dock_widget_buttons(self):
        self.rotate90_button = QPushButton("Rotate 90")
        self.rotate90_button.setStatusTip("The photo will be Rotated 90 degrees")
        self.rotate90_button.clicked.connect(partial(self.rotate, 90))
        self.rotate90_button.setFixedSize(QSize(200, 40))
        self.rotate90_button.setStyleSheet("font-size:20px;")

        self.rotate180_button = QPushButton("Rotate 180")
        self.rotate180_button.setStatusTip("The photo will be Rotated 180 degrees")
        self.rotate180_button.clicked.connect(partial(self.rotate, 180))
        self.rotate180_button.setFixedSize(QSize(200, 40))
        self.rotate180_button.setStyleSheet("font-size:20px;")

        self.flip_horizontal_button = QPushButton("Flip Horizontal")
        self.flip_horizontal_button.setStatusTip("The photo will be flipped horizontally")
        self.flip_horizontal_button.clicked.connect(partial(self.flip, -1, 1))
        self.flip_horizontal_button.setFixedSize(QSize(200, 40))
        self.flip_horizontal_button.setStyleSheet("font-size:20px;")

        self.flip_vertical_button = QPushButton("Flip Vertical")
        self.flip_vertical_button.setStatusTip("The photo will be flipped vertically")
        self.flip_vertical_button.clicked.connect(partial(self.flip, 1, -1))
        self.flip_vertical_button.setFixedSize(QSize(200, 40))
        self.flip_vertical_button.setStyleSheet("font-size:20px;")

        self.resize_half_button = QPushButton("Resize Half")
        self.resize_half_button.setStatusTip("The photo will be scaled to half the size")
        self.resize_half_button.clicked.connect(partial(self.flip, 0.5, 0.5))
        self.resize_half_button.setFixedSize(QSize(200, 40))
        self.resize_half_button.setStyleSheet("font-size:20px;")

    def add_dock_widget_buttons(self):
        self.dock_widget_widget = QWidget()
        layout = QVBoxLayout()
        layout.addWidget(self.rotate90_button)
        layout.addWidget(self.rotate180_button)
        layout.addStretch(10)
        layout.addWidget(self.flip_horizontal_button)
        layout.addWidget(self.flip_vertical_button)
        layout.addStretch(10)
        layout.addWidget(self.resize_half_button)
        layout.addStretch(90)

        self.dock_widget_widget.setLayout(layout)

        self.dock_widget.setWidget(self.dock_widget_widget)

        self.addDockWidget(Qt.DockWidgetArea.RightDockWidgetArea, self.dock_widget)

    def dock_widget_toggled_action(self):
        self.dock_widget.toggled_act = self.dock_widget.toggleViewAction()

        self.view_menu.addAction(self.dock_widget.toggled_act)

    def open_photo(self):
        file_name, _ = QFileDialog.getOpenFileName(self, "Open Photo", "",
                                                   "JPG (*.jpg *.jpeg);; PNG (*.png);; SVG (*.svg);; BMP (*.bmp)")
        if file_name:
            if not self.image.isNull():
                flag = True
                while flag:
                    user_ans = QMessageBox.question(self, "Warning",
                                                    "Do you want to save this photo before open new one?",
                                                    QMessageBox.StandardButton.Yes | QMessageBox.StandardButton.No |
                                                    QMessageBox.StandardButton.Cancel,
                                                    QMessageBox.StandardButton.Cancel)
                    if user_ans == QMessageBox.StandardButton.Yes:
                        if self.save_photo():
                            self.central_label.clear()
                            flag = False
                            self._open(file_name)
                    elif user_ans == QMessageBox.StandardButton.Cancel:
                        flag = False
                    elif user_ans == QMessageBox.StandardButton.No:
                        self.central_label.clear()
                        self._open(file_name)
                        flag = False
            else:
                self._open(file_name)

    def _open(self, file_name):
        try:
            self.image = QPixmap(file_name)
            img = self.image.scaled(self.central_label.size(), Qt.AspectRatioMode.IgnoreAspectRatio,
                                    Qt.TransformationMode.SmoothTransformation)
            self.central_label.setPixmap(img)
            self.last_image = self.image
        except:
            QMessageBox.warning(self, "Error", "Photo file is not valid.")

    def save_photo(self):
        if not self.image.isNull():
            filename, _ = QFileDialog.getSaveFileName(self, "Save Photo", "",
                                                      "JPG (*.jpg *.jpeg);; PNG (*.png);; SVG (*.svg);; BMP (*.bmp)")
            if filename:
                self.image.save(filename)
                QMessageBox.information(self, "Success", "Photo successfully saved")
                self.last_image = self.image
            else:
                QMessageBox.information(self, "Not Saved", "Photo name is not valid.", QMessageBox.StandardButton.Close,
                                        QMessageBox.StandardButton.Close)
        else:
            QMessageBox.warning(self, "Error", "There is no photo to save!", QMessageBox.StandardButton.Close,
                                QMessageBox.StandardButton.Close)

    def rotate(self, number):
        if not self.image.isNull():
            self.image = self.image.transformed(QTransform().rotate(number),
                                                mode=Qt.TransformationMode.SmoothTransformation)
            self.central_label.clear()
            img = self.image
            img.scaled(self.central_label.size(), Qt.AspectRatioMode.KeepAspectRatio,
                       Qt.TransformationMode.SmoothTransformation)
            self.central_label.setPixmap(img)
            if number == 180:
                self.set_full_screen()

    def set_full_screen(self):
        if not self.image.isNull():
            img = self.image
            img = img.scaled(self.central_label.size(), Qt.AspectRatioMode.IgnoreAspectRatio,
                             Qt.TransformationMode.SmoothTransformation)
            self.central_label.setPixmap(img)

    def flip(self, x, y):
        if not self.image.isNull():
            self.image = self.image.transformed(QTransform().scale(x, y),
                                                mode=Qt.TransformationMode.SmoothTransformation)
            self.central_label.clear()
            self.set_full_screen()

    def closeEvent(self, event):
        if self.image.isNull():
            event.accept()
        else:
            user_ans = QMessageBox.question(self, "Quit the app", "Do you want to quit?\nIf you have unsaved changes"
                                                                  ",that will be disappeared!",
                                            QMessageBox.StandardButton.Yes, QMessageBox.StandardButton.Cancel)
            if user_ans == QMessageBox.StandardButton.Yes:
                event.accept()
            else:
                event.ignore()

    def reset_photo(self):
        if not self.image.isNull():
            user_ans = QMessageBox.question(self, "Reset Photo", "Do you want to reset the photo to last saved?"
                                                                 "\nIf you have unsaved changes, that will be disappeared!",
                                            QMessageBox.StandardButton.Yes | QMessageBox.StandardButton.Cancel,
                                            QMessageBox.StandardButton.Cancel)
            if user_ans == QMessageBox.StandardButton.Yes:
                self.image = self.last_image
                self.central_label.clear()
                self.set_full_screen()

    def clear_image(self):
        if not self.image.isNull():
            user_ans = QMessageBox.question(self, "Clear the image", "Do you want to clear the image?"
                                                                     "\nIf you have unsaved changes, that will be disappeared!",
                                            QMessageBox.StandardButton.Yes | QMessageBox.StandardButton.Cancel,
                                            QMessageBox.StandardButton.Cancel)
            if user_ans == QMessageBox.StandardButton.Yes:
                self.image = QPixmap()
                self.last_image = self.image
                self.central_label.clear()

if __name__ == '__main__':
    app = QApplication([])
    photo_editor = PhotoEditor()
    app.setAttribute(Qt.ApplicationAttribute.AA_DontShowIconsInMenus)
    exit(app.exec())
