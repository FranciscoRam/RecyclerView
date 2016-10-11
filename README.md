# RecyclerView
------
**Francisco Ramirez Garnica**

**Aplicacione Moviles**

---------

A diferencia del ListView, GridView, etc� el RecyclerView se dedica �nicamente a lo que su nombre indica, reciclar, reutilizar recursos y evitar el uso reiterado del costoso findViewById, no se preocupa del aspecto visual, para ello est� el LayoutManager, 

Una clase una tarea, esa es la filosof�a que sigue la API del RecyclerView, un paquete de clases  internas cada una con una responsabilidad:
* Adapter
* ViewHolder
* LayoutManager
* ItemDecoration
* ItemAnimator

**Adapter**

Esta clase se encarga de crear las Views necesarias para cada elemento del RecyclerView, adem�s, est� muy unida al ViewHolder, teniendo que ser indicado en la declaraci�n de la clase, muchos pensar�is que esto no es nuevo, que Google ya aconsejara este patr�n tiempo atr�s, esta vez fuerza a utilizarlo, teniendo que ser indicado en la implementaci�n del Adapter, un paso adelante, sin duda.

El m�todo OnCreateViewHolder inicializa el ViewHolder
```  java
@Override
public ViewHolder onCreateViewHolder(ViewGroup parentViewGroup, int i) {
 
    View rowView = LayoutInflater.from (parentViewGroup.getContext())
        .inflate(R.layout.list_basic_item, parentViewGroup, false);
 
    return new ViewHolder (rowView);
}
```
El m�todo onBindViewHolder(ViewHolder viewholder, int position) se usa para configurar el contenido de las Views
``` java
@Override
public void onBindViewHolder(ViewHolder viewHolder, int position) {
 
    final SampleModel rowData = sampleData.get(position);
    viewHolder.textViewSample.setText(rowData.getSampleText());
    viewHolder.itemView.setTag(rowData);
}
```
**ViewHolder**

Como ven�a diciendo, el patr�n ViewHolder no es nada nuevo, de hecho Google, lo lleva recomendando desde hace tiempo, se puede pensar en el como un cache de las vistas, pudiendo reutilizarlas en vez de crearlas nuevamente.
``` java
@Override
public static class ViewHolder extends RecyclerView.ViewHolder {
 
     private final TextView textViewSample;
 
     public ViewHolder(View itemView) {
         super(itemView);
 
         textViewSample = (TextView) itemView.findViewById(
             R.id.textViewSample);
     }
 }
 ```
**LayoutManager**

El LayoutManager se encarga del layout de todas las vistas dentro del RecyclerView, concretando con el LinearLayoutManager, permite entre otros acceder a elementos mostrados en la pantalla como el primer elemento, �ltimo, o por ejemplo, el �ltimo completamente visible, esto de forma horizontal o vertical, en el ejemplo se ha utilizado la disposici�n en vertical.
``` java
LinearLayoutManager mLayoutManager = new LinearLayoutManager(this);
recyclerView.setLayoutManager(mLayoutManager);
```

**ItemDecorator**

Otro eslab�n importante, son los llamados ItemDecorator, estos permiten modificar los elementos del RecycleView, este adem�s, ofrece adem�s ofrece un elemento llamado insets (m�rgenes) que pueden aplicarse a las vistas sin necesidad de modificar los par�metros del layout.

En el ejemplo, se muestra como se usan los ItemDecorators para dibujar un peque�o Divider entre los elementos del RecyclerView:
``` java
package saulmm.com.recyclerviewproject;
 
import android.content.Context;
import android.content.res.TypedArray;
import android.graphics.Canvas;
import android.graphics.drawable.Drawable;
import android.support.v7.widget.RecyclerView;
import android.view.View;
 
public class SampleDivider extends RecyclerView.ItemDecoration {
 
    private static final int[] ATTRS = { android.R.attr.listDivider };
 
    private Drawable mDivider;
 
    public SampleDivider(Context context) {
        TypedArray a = context.obtainStyledAttributes(ATTRS);
        mDivider = a.getDrawable(0);
        a.recycle();
 
    }
 
    @Override
    public void onDrawOver(Canvas c, RecyclerView parent) {
 
        int left = parent.getPaddingLeft();
        int right = parent.getWidth() - parent.getPaddingRight();
 
        int childCount = parent.getChildCount();
 
        for (int i = 0; i < childCount; i++) {
 
            View child = parent.getChildAt(i);
 
            RecyclerView.LayoutParams params = (RecyclerView.LayoutParams) child
                    .getLayoutParams();
 
            int top = child.getBottom() + params.bottomMargin;
            int bottom = top + mDivider.getIntrinsicHeight();
 
            mDivider.setBounds(left, top, right, bottom);
            mDivider.draw(c);
        }
    }
}
```

**ItemAnimator**
La clase ItemAnimator como su nombre indica, anima el RecyclerView cuando se a�ade y se elimina un elemento, el RecyclerView utiliza un ItemAnimator por defecto.

El RecyclerView ha de saber cuando se inserta un elemento o se elimina, con elementos como ListViews, GridViews, etc� esto se consegu�a llamando al m�todo notifyDataSetChanged(), a nivel de performance, es bastante costoso, ya que redibuja todos los items en el layout, lo propio con el RecyclerView es usar el m�todo notifyItemInserted() para a�adir, y notifyItemRemoved() para eliminar, actualizando solo la parte apropiada.

**Mas articulos sobre RecyclerView**

[Articulo de referencia](http://androcode.es/2014/10/un-vistazo-rapido-al-nuevo-recyclerview/)

[Artivulo 1](https://danielme.com/2015/08/15/android-recycler-view-listas/)

[Articulo 2](https://geekytheory.com/recyclerview-el-heredero-del-listview/)

[Articulo 3](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)

[Articulo 4](http://www.sgoliver.net/blog/controles-de-seleccion-v-recyclerview/)

[Articulo 5 +](http://www.hermosaprogramacion.com/2015/02/android-recyclerview-cardview/)

