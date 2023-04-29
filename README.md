# BezierCurve

using System;
using System.Collections;
using System.Collections.Generic;
using Unity.VisualScripting;
using UnityEngine;

public class Bezier : MonoBehaviour
{
    //Create an empty GameObject in the scene and add a LineRenderer to it
    //Name it Bezier for an easier comprehension
    //Add this script to Bezier
    //Assign the LineRenderer to the lineRenderer variable
    //Create 4 points in the scene
    //Name them Point0, Point1, Point2 and Point3
    //Assign them to the respectives variables point0, point1, point2 and point3

    //To be able to switch between the 3 types of curves you probably would need an enum
    //You may want to control the "quality" of the curve
    //If so, make numPoints public and change positions size in the start method to : new Vector3[numPoints];

    public LineRenderer lineRenderer;
    public Transform point0, point1, point2, point3;

    private int numPoints = 50;
    private Vector3[] positions = new Vector3[50];


    private void Start()
    {
        lineRenderer.positionCount = numPoints;
        //DrawlinearCurve();
        //DrawQuadraticCurve();
        DrawCubicCurve();
    }

    private void Update()
    {
        //DrawlinearCurve();
        //DrawQuadraticCurve();
        DrawCubicCurve();
    }

    private void DrawlinearCurve()
    {
        for (int i = 1; i < numPoints + 1; i++)
        {
            float t = i / (float)numPoints;
            positions[i - 1] = CalculateLinearBezierPoint(t, point0.position, point1.position);
        }

        lineRenderer.SetPositions(positions);
    }

    private void DrawQuadraticCurve()
    {
        for (int i = 1; i < numPoints + 1; i++)
        {
            float t = i / (float)numPoints;
            positions[i - 1] = CalculateQuadraticBezierPoint(t, point0.position, point1.position, point2.position);
        }
        lineRenderer.SetPositions(positions);
    }

    private void DrawCubicCurve()
    {
        for (int i = 1; i < numPoints + 1; i++)
        {
            float t = i / (float)numPoints;
            positions[i - 1] = CalculateCubicBezierPoint(t, point0.position, point1.position, point2.position, point3.position);
        }
        lineRenderer.SetPositions(positions);
    }

    private Vector3 CalculateLinearBezierPoint(float t, Vector3 p0, Vector3 p1)
    {
        // return : p0 + t(p1 - p0)

        return p0 + t * (p1 - p0);
    }

    private Vector3 CalculateQuadraticBezierPoint(float t, Vector3 p0, Vector3 p1, Vector3 p2)
    {
        // return : (1-t)^2 p0 + 2(1-t)tP1 + t^2p2
        //            u            u        tt
        //           uu * p0 + 2 * u * t * p1 + tt * p2

        float u = 1 - t;
        float tt = t * t;
        float uu = u * u;

        Vector3 p = uu * p0;
        p += 2 * u * t * p1;
        p += tt * p2;

        return p;
    }

    private Vector3 CalculateCubicBezierPoint(float t, Vector3 p0, Vector3 p1, Vector3 p2, Vector3 p3)
    {
        // return : (1 - t)^3p0 + 3(1 - t)^2 tp1 + 3(1 - t)t^2p2 + t^3p3
        //            uuu            uu              u   tt      ttt

        float u = 1 - t;
        float tt = t * t;
        float uu = u * u;
        float uuu = uu * u;
        float ttt = tt * t;
        Vector3 p = uuu * p0;
        p += 3 * uu * t * p1;
        p += 3 * u * tt * p2;
        p += ttt * p3;

        return p;
    }
}
