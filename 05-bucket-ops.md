Buckets are versatile file organization tools.

### 1. Bucket metadata

Unlike the `File` object, buckets don't need any fancy DOM rendering or
cryptography. So they're just simple objects with a few informational
properties. We already know how to create a new bucket, so let's grab a list of
all the buckets we've made so far, and log the bucket info to console:

```javascript
storj.getBucketList(function (error, metadata) {
  if (error) {
    return console.log(error)
  }
  metadata.forEach(function (item) {
    console.log('----BEGIN BUCKET----')
    console.log('Bucket name', item.name)
    console.log('Bucket id', item.id)
    console.log('Bucket storage in GB', item.storage)
    console.log('Bucket transfer in GB', item.transfer)
    console.log('Bucket created date', item.created)
    console.log('Bucket public permissions', item.publicPermissions)
    item.pubkeys.forEach(function (key) {
      console.log('A key:', key)
    })
    console.log('-----END BUCKET-----')
  })
})
```

The bucket object contains info about the bucket, like `id` and `name`, as well
as information about bucket usage like how many gigabytes are in the bucket
(`storage`) and how many gigabytes of bandwidth have been used this month
(`bandwidth`).

We can also get metadata for a single bucket:

```javascript
storj.getBucket(bucketID, function (error, metadata) {
  if (error) {
    return console.log(error)
  }
  console.log('Bucket name', metadata.name)
  // Etc.
})
```

### 2. Deleting Buckets

Sometimes you just gotta throw some things away. So `storj.deleteBucket()` does
exactly what it says it does.

```javascript
storj.deleteBucket(function (error) {
  if (error) {
    return console.log(error)
  }
})
```

There's no confirmation, so be very sure you want to delete things before you
go deleting things.

### 3. Listing Files in a Bucket

If you know the ID of your bucket, you can get a list of all the file metadata
in it. File metadata is represented as simple objects until you make a `File`
object to download it.

```javascript
storj.getFileList(bucketID, function (error, fileMetadata) {
  if(error) {
    return console.log(error)
  }
  fileMetadata.forEach(function (item) {
    console.log('----BEGIN FILE----')
    console.log('filename:', item.filename)
    console.log('id:', item.id)
    console.log('mimetype:', item.mimetype)
    console.log('size in bytes:', item.size)
    console.log('-----END FILE-----')
  })
})
```

### 4. Making a Public Bucket

Sometimes it'd be nice to have publicly accessible buckets. Maybe you're making
a portfolio or an image gallery. Maybe you want to send some files to your
friends in a hurry. Maybe you just feel like paying for file storage for
strangers out of the goodness of your heart.

Whatever motivates you, public buckets are a powerful tool. Clients don't have
to authenticate to interact with public buckets. Buckets can be public push,
public pull, or both. Setting up a public bucket is easy:

```javascript
var perms = [
  'PULL', // Allow anyone to download, defaults to false
  'PUSH'  // Allow anyone to upload, defaults to false
]

storj.makePublic(bucketID, perms, function (error) {
  if (error) {
    return console.log(error)
  }
})
```

Keep in mind that bucket limits are not currently implemented. So if you set a
bucket to public, keep an eye on usage. Better management features are coming
soon.

### Next Up:

We're going to dive into the types of [keys](06-keys.md) in Storj.
