import nbformat
from nbconvert.preprocessors import ExecutePreprocessor
import gc

def run_notebook(notebook_path, output_path):
    with open(notebook_path) as f:
        nb = nbformat.read(f, as_version=4)
    
    ep = ExecutePreprocessor(timeout=600, kernel_name='python3')
    
    for i, cell in enumerate(nb.cells):
        if cell.cell_type == 'code':
            try:
                ep.preprocess_cell(cell, {'metadata': {}}, i)
            except Exception as e:
                print(f"Exception in cell {i}: {e}")
            finally:
                # Clear variables and invoke garbage collection after each cell
                globals().clear()
                gc.collect()
    
    with open(output_path, 'w', encoding='utf-8') as f:
        nbformat.write(nb, f)

if __name__ == "__main__":
    notebook_path = "path_to_your_notebook.ipynb"
    output_path = "path_to_save_executed_notebook.ipynb"
    run_notebook(notebook_path, output_path)
