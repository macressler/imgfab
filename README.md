imgfab
======

Because your Facebook photos are so 2D!

How it works:

 - Facebook connect => List of images & descriptions from a user, saved as JSON
 - `tasks/gather_data.py`: Downloads all the images in a temporary directory
 - `tasks/create_model.py`: Creates a blender model from the directory
 - `tasks/upload_to_sketchfab.py`: Uploads the model to http://sketchfab.com


Blockers
========
 - Sphere
 - New name: picfab.co / pixfab.co ?
 - ratio photos
 - shadeless

 - enboyer piece
 - oauth avec pa / simplfy signup
 - upload annotations


TODO
====

 - Adapt plane size to the photo ratio / crop differently
 - New layouts (sphere, museum room)
 - Background image
 - Set sketchfab name from album name
 - Make 2 mrq queues to avoid runing out of greenlets if load grows
 - Show FB album photo count (+ thumbnails?)
 - Much more exception handling
 - User-supplied Sketchfab API keys
 - Other photo sources: Twitter, Instagram, Flickr, URL list
 - Nicer 3D rendering/lighting
 - Upload with annotations
 - Favicon / Logo
 - Share result


Host your own instance on Heroku!
=================================


```
# Create an heroku app with this
heroku apps:create
heroku config:set BLENDER_PATH=build/blender-2.73a-linux-glibc211-x86_64/blender-softwaregl

# Add your API keys
heroku config:set SKETCHFAB_API_KEY=x SOCIAL_AUTH_FACEBOOK_KEY=x SOCIAL_AUTH_FACEBOOK_SECRET=x SOCIAL_AUTH_SKETCHFAB_KEY=x SOCIAL_AUTH_SKETCHFAB_SECRET=x

# Add MongoDB & Redis (for storage & task queue)
heroku addons:add rediscloud
heroku addons:add mongolab
```

Run imgfab/instamuseum locally
==============================

To test locally you can pull the config variables:

```

# If you have access, get config from prod.
# If not, export your own values!
export MONGOLAB_URI=`heroku config:get -a imgfab MONGOLAB_URI`
export REDISCLOUD_URL=`heroku config:get -a imgfab REDISCLOUD_URL`
export SKETCHFAB_API_KEY=`heroku config:get -a imgfab SKETCHFAB_API_KEY`
export SOCIAL_AUTH_FACEBOOK_KEY=`heroku config:get -a imgfab SOCIAL_AUTH_FACEBOOK_KEY`
export SOCIAL_AUTH_FACEBOOK_SECRET=`heroku config:get -a imgfab SOCIAL_AUTH_FACEBOOK_SECRET`
export SOCIAL_AUTH_INSTAGRAM_ID=`heroku config:get -a imgfab SOCIAL_AUTH_INSTAGRAM_ID`
export SOCIAL_AUTH_INSTAGRAM_SECRET=`heroku config:get -a imgfab SOCIAL_AUTH_INSTAGRAM_SECRET`
export SOCIAL_AUTH_SKETCHFAB_KEY=`heroku config:get -a imgfab SOCIAL_AUTH_SKETCHFAB_KEY`
export SOCIAL_AUTH_SKETCHFAB_SECRET=`heroku config:get -a imgfab SOCIAL_AUTH_SKETCHFAB_SECRET`

# If you installed blender with homebrew:
export BLENDER_PATH=/opt/homebrew-cask/Caskroom/blender/2.76b/blender-2.76b-OSX_10.6-x86_64/blender.app/Contents/MacOS/blender

# Then run a worker, a dashboard & the flask app:
mrq-worker highpriority default &
mrq-dashboard &
python flaskapp/app.py
```