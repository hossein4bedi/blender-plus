# -----------------------------
# Calculates o Rotation based on the direction of the longest edge.
# Application: when you seprate some parts of an object, the newly created objects do not have a correct rotation. this script assigns a rotation to every selected object based on the direction of the longest edge.
# This is the Last version and Works. V2.
# -----------------------------
import bpy
import bmesh
from mathutils import Vector, Matrix
import math
from mathutils import Vector, Quaternion
from mathutils import Vector
import bpy
from mathutils import Matrix, Euler, Quaternion
# ------------------------------------

def rotate_object(obj, angle_degrees, axis='Z'):
    ''' rotates an object '''
    from math import radians
    from mathutils import Matrix

    # local rotation about axis
    obj.rotation_euler = (obj.rotation_euler.to_matrix() @ Matrix.Rotation(radians(angle_degrees), 3, axis)).to_euler()
    

def rotate_object_radians(obj, angle_radians, axis='Z'):
    ''' rotates an object '''
    from math import radians
    from mathutils import Matrix

    # local rotation about axis
    obj.rotation_euler = (obj.rotation_euler.to_matrix() @ Matrix.Rotation(angle_radians, 3, axis)).to_euler()
    
    
# ------------------------------------
# Calculate Rotation Based On Two Points
#def getRotationByTwoPoints(forwardVec, P1, P2)
#    
#    # normalize B
#    B.normalize()

#    # cross and dot products of A and B
#    C = forwardVec.cross(B)
#    D = forwardVec.dot(B)

#    # create and normalize a quaternion
#    Q = Quaternion((D+1, C[0], C[1], C[2]))
#    Q.normalize()
#    
#    return Q
# ------------------------------------
# ------------------------------------
# ------------------------------------
pi_2 = math.pi / 2

bpy.ops.object.mode_set( mode = 'OBJECT' )

sel_objs = [obj for obj in bpy.context.selected_objects if obj.type == 'MESH']

for obj in sel_objs: 
    obj.select_set(False) 

count = 0
while len(sel_objs) >= 1:   
    count = count + 1
    obj1 = sel_objs.pop()  
    for i in [0, 1, 2]:
        bpy.ops.object.mode_set( mode = 'OBJECT' )
        
        obj1.select_set(True) 
    #    bpy.context.view_layer.objects.active = obj1
    #    bpy.context.scene.tool_settings.use_transform_data_origin = False
    #    bpy.context.scene.transform_orientation_slots[0].type = 'NORMAL'
    #    bpy.ops.object.mode_set( mode = 'EDIT' )
    #    bpy.ops.mesh.select_mode( type = 'FACE' )
    #    bpy.ops.mesh.select_all( action = 'DESELECT' )
    #    bpy.ops.mesh.select_face_by_sides(number=4, type='GREATER')
    #    bpy.ops.transform.create_orientation(name='tmp', use_view=False, use=False, overwrite=True)
    #    bpy.context.scene.transform_orientation_slots[0].type = 'tmp'
        
    #    bpy.context.scene.tool_settings.use_transform_data_origin = True
    #    bpy.ops.transform.transform(mode='ALIGN', orient_type='tmp', orient_matrix_type='tmp')
    #    bpy.context.scene.tool_settings.use_transform_data_origin = False
    #    bpy.context.scene.transform_orientation_slots[0].type = 'LOCAL'
        
        
        mat = obj1.matrix_world
        localX = Vector((mat[0][0],mat[1][0],mat[2][0]))
        localY = Vector((mat[0][1],mat[1][1],mat[2][1]))
        localZ = Vector((mat[0][2],mat[1][2],mat[2][2]))
    #    print(localZ)
        
        # Object faces are in data.polygons
    #    p = obj1.data.polygons[2]
        
        OWMatrix = obj1.matrix_world
        maxLen = 0
        maxLenEedge = None
        maxLenDir = None
        maxLenV0 = None
        maxLenV1 = None
        
        for e in obj1.data.edges :
            
            v0 = e.vertices[0]
            v1 = e.vertices[1]
    #        print(obj1.data.vertices[v0].co)
            v0Pos = OWMatrix @ obj1.data.vertices[v0].co
            v1Pos = OWMatrix @ obj1.data.vertices[v1].co
            eDir = (v0Pos - v1Pos)
            edgeLen = eDir.length
            
            if edgeLen > maxLen:
                maxLen = edgeLen
                maxLenEedge = e
                maxLenDir = eDir
                maxLenV0 = v0Pos
                maxLenV1 = v1Pos
                
    #            print(v0Pos)

        world_X = Vector((1,0,0))
        
        prevDist = (maxLenV0 - world_X).length
        dir = (maxLenDir).normalized()
    #    print(dir)
    #    axisForward = Vector([1, 0, 0]).normalized()
    #    z = localZ # 
    #    axis = z.cross(maxLenDir)
    #    yaw = p.normal.angle(localX)
        
        angle_to_local_axis_x = (OWMatrix @ maxLenDir).angle(localX)
        
    #    print("angle_to_local_axis_x: ")
    #    print(angle_to_local_axis_x)
        
        coeff = 1
        if angle_to_local_axis_x > pi_2:
            coeff = -1
        
    #    yaw = math.acos(dir[0]) * coeff
        dir_w = OWMatrix @ maxLenDir
    #    print('dir_w')
    #    print(dir_w[0])
        yaw = math.acos(dir[0]) 
        angle_to_local_X = dir.angle(localX)
        
        if count % 10 == 0:
            print(count)

        
#        print(obj1.name)
#        print(angle_to_local_X)
        if angle_to_local_X < 0.022 or angle_to_local_X > 3.12:
            obj1.hide_set(True)
            continue

        if i == 2:
            continue
        
        bpy.ops.object.transform_apply( location = False, rotation = True, scale = False )
        if i == 1:
            yaw = -yaw
            
        
        
        
    #    

    #    bpy.ops.object.mode_set( mode = 'EDIT' )
    #    bpy.ops.mesh.select_mode(type='EDGE')
    #    bpy.ops.mesh.select_all(action='SELECT')
    #    
    #    bpy.ops.transform.rotate(value=yaw)
    ###    bpy.ops.transform.rotate(value=yaw, orient_axis='Z')
    ##    

    #    bpy.ops.object.mode_set( mode = 'OBJECT' )
        rotate_object_radians(obj1, -yaw)
    #    obj1.rotation_euler[2] = yaw
        bpy.ops.object.transform_apply( location = False, rotation = True, scale = False )
    #    obj1.rotation_euler[2] = -yaw
        rotate_object_radians(obj1, yaw)
    #    bpy.ops.transform.rotate(value=-yaw)
    #        obj1.rotation_euler[2] = yaw
    #        bpy.ops.object.transform_apply( location = False, rotation = True, scale = False )
    #        obj1.rotation_euler[2] = -yaw


    #        if i==0:
    #            bpy.ops.object.transform_apply( location = False, rotation = True, scale = False )
    #    bpy.ops.transform.rotate(value=yaw)


        
        
    #    v0 = maxLenEedge.vertices[0]
    #    v1 = maxLenEedge.vertices[1]
    ##        print(obj1.data.vertices[v0].co)
    #    v0Pos = obj1.data.vertices[v0].co
    #    v1Pos = obj1.data.vertices[v1].co
    #    
    #    newDist = (v0Pos - localX).length
    #    
    #    if newDist > prevDist:
    #        yaw = math.acos(dir[1]) 
    #        obj1.rotation_euler[2] = yaw
    #        bpy.ops.object.transform_apply( location = False, rotation = True, scale = False )
    #        obj1.rotation_euler[2] = -yaw
    #    else:
    #        bpy.ops.object.transform_apply( location = False, rotation = True, scale = False )
    #        obj1.rotation_euler[2] = -yaw

        
        obj1.select_set(False)
