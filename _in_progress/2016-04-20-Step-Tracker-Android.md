---
layout: post
title: How to track steps and calculate running distance - Android
author: Lewis Gavin
comments: true
tags:
- android
- programming
- java
---

I am currently in the process of uplifting an android application I built in my spare time. Between finishing university and starting work I had a fair amount of free time, which I used to hit the gym in attempt to become Hugh Jackman. I found myself trying numerous apps and most of them just contained too much "stuff" and were really bloated. All I wanted was a simple app to track my exercises and maybe show me a graph of my progress. This inspired me to use the rest of my free time to start my own android app. 

The app I made was called Gymify. It is not available on the app store (yet) but at the time it was a simple list based application (see my previous post on my List implementation here: [ListView Adapters](../))where a user could add routines and then work through exercises in a routine and tick them off. 

Once my crossover period between finishing uni and starting work had ended I found that I became too busy and progress on the app slowed. Recently I have tried to get myself back into the habit of doing a little bit of coding in my spare time, even if its just 10 minutes a night. This has allowed me to escape work based problems and also focus on different programming techniques using different technologies than those used at work.

One of the enhancements I made to the app recently was to introduce the capability to store cardio based activities. For this I wanted to include tracking the amount of time, steps and distance per workout. This led me to think about how distance could be tracked if, for example, the user was using a treadmill instead of running outside. GPS tracking wouldn't work well in this situation, as the subject is not really moving geographically at all. This post will explore how with a simple library, step tracking can be added to an application, and then show how I used this information to approximate running distances.

## Tracking Steps using SensorEventListener

You'd think tracking steps with some degree of accuracy would be rather difficult to do, but Google couldn't have made it easier with thei android library *SensorEventListener*. By simply implementing this listener within a class and overriding the two methods **onSensorChanged** and **onAccuracyChanged** you can start tracking steps. 

Start by initialising a SensorManager and a Sensor.

~~~java
import android.hardware.Sensor;
import android.hardware.SensorEvent;
import android.hardware.SensorEventListener;
import android.hardware.SensorManager;
import android.app.Activity;

public class StepActivity extends Activity implements SensorEventListener{
	SensorManager sManager = (SensorManager) getSystemService(Context.SENSOR_SERVICE);
	Sensor stepSensor = sManager.getDefaultSensor(Sensor.TYPE_STEP_DETECTOR);

	...

}
~~~

These Sensor is initialised as a *TYPE_STEP_DETECTOR* meaning it will determine when a step has been taken. They will be used to register the listener to the activity and deregister it when the activity stops. We can do this by overriding the Activities **onResume()** and **onStop()** functions.

~~~java
	@Override
    protected void onResume() {

        super.onResume();

        sManager.registerListener(this, stepSensor, SensorManager.SENSOR_DELAY_FASTEST);

    }

    @Override
    protected void onStop() {
        super.onStop();
        sManager.unregisterListener(this, stepSensor);
    }

~~~

Now we have initialised the SensorManager and Sensor and have the Sensor registered as a listener within the activity, we now need to implement the **onSensorChanged** function that will be triggered by a *SensorEvent* whenever there is a change to the Sensor we registered, in our case the **TYPE_STEP_DETECTOR**.

~~~java
	private long steps = 0;

	@Override
    public void onSensorChanged(SensorEvent event) {
        Sensor sensor = event.sensor;
        float[] values = event.values;
        int value = -1;

        if (values.length > 0) {
            value = (int) values[0];
        }


        if (sensor.getType() == Sensor.TYPE_STEP_DETECTOR) {
            steps++;
        }
    }
~~~
