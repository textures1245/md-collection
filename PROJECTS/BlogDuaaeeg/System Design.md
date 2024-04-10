[[BlogDuaaeeg Overview]]

References
- [[App Requirement]]
- [[PROJECTS/BlogDuaaeeg/System Analysis|System Analysis]]

**Contents**
#BlogDuaaeeg-Class-Diagram 
#BlogDuaaeeg-Usecases

## Class Diagram 
- Based on #BlogDuaaeeg-ERD structure we designed. We can diagram application services from #BlogDuaaeeg-feat-reqs  that involve with Database by following.

```merm
classDiagram
    class User {
        -userId: int
        -username: string
        -email: string
        -password: string
        +registerUser(userInfo)
        +authenticateUser(credentials)
        +resetPassword(email)
    }

	class UserProfile {
		-firstName: string
        -lastName: string
        -bio: string
        -profilePicture: string
        +updatePersonalInfo(info)
        +updateProfilePicture(picture)
	}

    class Post {
        -postId: int
        -title: string
        -content: string
        -createdAt: date
        -updatedAt: date
        -published: boolean
        -userId: int
        +createPost(userId, postData)
        +importMarkdownPost(userId, file)
        +saveAsDraft(userId, draftId)
        +schedulePublishing(userId, publishDate)
        +publishPost(userId)
        +previewPost(userId)
        +updatePost(userId, updatedData)
        +unpublishPost(userId)
    }

    class Tag {
        -tagId: int
        -name: string
    }

    class Category {
        -categoryId: int
        -name: string
    }

    class PostTag {
        -postId: int
        -tagId: int
    }

    class PostCategory {
        -postId: int
        -categoryId: int
    }

    class Comment {
        -commentId: int
        -content: string
        -createdAt: date
        -userId: int
        -postId: int
        +commentOnPost(userId, postId, commentData)
    }

    class Like {
        -likeId: int
        -userId: int
        -postId: int
        +likePost(userId, postId)
    }

    class Content {
        -publicationId: int
        -name: string
        -description: string
        +submitToPublication(userId, postId)
    }

    class PublicationPost {
        -publicationId: int
        -postId: int
		+shareOnSocial(userId, postId, platform)
    }

    User "1" --o "0..*" Post : creates
    User "1" --o "0..*" Comment : writes
    User "1" --o "0..*" Like : "likes"
    User "1" --o "0..*" Content : "submits to"
    User "1" --o "1" UserProfile : "updated"
    Post "0..*" --o "0..*" PostTag : tagged
    Post "0..*" --o "0..*" PostCategory : categorized
    Tag "1" --o "0..*" PostTag : tagged
    Category "1" --o "0..*" PostCategory : categorized
    Comment "0..*" --o "1" Post : "commented on"
    Like "0..*" --o "1" PublicationPost : "liked"
    Content "1" --o "1" PublicationPost : "publishes"
	Post "1" --o "1" Content : "submits to"

    class ContentDiscoveryService {
        +searchPosts(query, filters)
		+searchByPostTag(postTagId)
		+searchByPostCategory(postCategoryId)
        +getFeaturedPosts()
        +getRecommendedPosts(userId)
    }

    class AnalyticsService {
        +getPostAnalytics(userId, postId)
        +integrateTool(toolName, config)
        +googleAnalytics()
    }

    ContentDiscoveryService "1" --o "0..*" PublicationPost : "discoveried"
    AnalyticsService "1" --o "0..*" PublicationPost : "analytics"
	PostTag "0..1" --o "1" ContentDiscoveryService : "sources"
	PostCategory "0..1" --o "1" ContentDiscoveryService : "sources"
```

## Use-cases
#BlogDuaaeeg-Usecases 
- Based on #BlogDuaaeeg-Class-Diagram for interface relationship and #BlogDuaaeeg-key-feats core service for application. We can design **Service And User User-cases** following "Sequence Diagram" visualization

1. **User Authentication and Registration**
```merm
sequenceDiagram
    Actor User
    participant Server
    participant UserManagementService
    participant UserEntity

	rect rgb(200, 200, 255, 0.5)
    User->>Server: POST /register (userInfo)
    Server->>UserManagementService: registerUser(userInfo)
    UserManagementService->>UserEntity: createUser(userInfo)
    UserEntity-->>UserManagementService: userCreated
    UserManagementService-->>Server: registrationSuccessful
    Server-->>User: registrationSuccessful
    end

	rect rgb(240, 200, 255, 0.5)
    User->>Server: POST /login (credentials)
    Server->>UserManagementService: authenticateUser(credentials)
    UserManagementService->>UserEntity: getUser(credentials)
    UserEntity-->>UserManagementService: userFound
    UserManagementService-->>Server: authenticationSuccessful
    Server-->>User: authenticationSuccessful
    end
```
2. **User Profiles**
```merm
sequenceDiagram
    actor User
    participant Server
    participant UserProfileService
    participant UserEntity
    
	rect rgb(200, 200, 255, 0.5)
    User->>Server: PUT /users/{userId} (info)
    Server->>UserProfileService: updatePersonalInfo(userId, info)
    UserProfileService->>UserEntity: getUserById(userId)
    UserEntity-->>UserProfileService: userFound
    UserProfileService->>UserEntity: updateUser(userId, info)
    UserEntity-->>UserProfileService: userUpdated
    UserProfileService-->>Server: infoUpdated
    Server-->>User: infoUpdated
    end

	rect rgb(240, 200, 255, 0.5)
    User->>Server: PUT /users/{userId}/picture (picture)
    Server->>UserProfileService: updateProfilePicture(userId, picture)
    UserProfileService->>UserEntity: getUserById(userId)
    UserEntity-->>UserProfileService: userFound
    UserProfileService->>UserEntity: updateUserPicture(userId, picture)
    UserEntity-->>UserProfileService: pictureUpdated
    UserProfileService-->>Server: pictureUpdated
    Server-->>User: pictureUpdated
    end
```
3. **Content Creation**     
	1. Create Post
```merm
sequenceDiagram
    actor User
    participant Server
    participant ContentCreationService
    participant ContentPublishingService
    participant PostEntity

    User->>Server: POST /posts (userId, postData)
    Server->>ContentCreationService: createPost(userId, postData)
    ContentCreationService->>UserEntity: getUserById(userId)
    UserEntity-->>ContentCreationService: userFound
    ContentCreationService->>PostEntity: createPost(userId, postData)
    PostEntity-->>ContentCreationService: postCreated
    ContentCreationService-->>Server: postCreated
    Server-->>User: postCreated
```
	2. Publish Post
```merm
sequenceDiagram
    actor User
    participant Server
    participant ContentCreationService
    participant ContentPublishingService
    participant PostEntity

    User->>Server: PUT /posts/{postId}/publish (userId)
    Server->>ContentPublishingService: publishPost(userId, postId)
    ContentPublishingService->>UserEntity: getUserById(userId)
    UserEntity-->>ContentPublishingService: userFound
    ContentPublishingService->>PostEntity: getPostById(postId)
    PostEntity-->>ContentPublishingService: postFound
    ContentPublishingService->>PostEntity: publishPost(userId, postId)
    PostEntity-->>ContentPublishingService: postPublished
    ContentPublishingService-->>Server: postPublished
    Server-->>User: postPublished
```


2. **Content Publishing**
```merm
sequenceDiagram
    participant User
    participant Server
    participant Content
    participant User
    participant PostEntity
    participant PublicationEntity
    participant PublicationPostEntity

    User->>Server: POST /publications/{publicationId}/posts (userId, postId)
    Server->>Content: submitToPublication(userId, postId, publicationId)
    Content->>UserEntity: getUserById(userId)
    UserEntity-->>Content: userFound
    Content->>PostEntity: getPostById(postId)
    PostEntity-->>Content: postFound
    Content->>PublicationEntity: getPublicationById(publicationId)
    PublicationEntity-->>Content: publicationFound
    Content->>PublicationPostEntity: createPublicationPost(publicationId, postId)
    PublicationPostEntity-->>Content: publicationPostCreated
    Content-->>Server: postSubmitted
    Server-->>User: postSubmitted
```
2. **Content Distribution**
- Submit posts to publications within the platform
- Social sharing buttons (Twitter, Facebook, LinkedIn)
3. **User Engagement**
	1. Like post
```merm
sequenceDiagram
    actor User
    participant Server
    participant UserEngagementService
    participant LikeEntity
    participant CommentEntity
    participant UserEntity
    participant PostEntity

    User->>Server: POST /posts/{postId}/likes (userId)
    Server->>UserEngagementService: likePost(userId, postId)
    UserEngagementService->>UserEntity: getUserById(userId)
    UserEntity-->>UserEngagementService: userFound
    UserEngagementService->>PostEntity: getPostById(postId)
    PostEntity-->>UserEngagementService: postFound
    UserEngagementService->>LikeEntity: createLike(userId, postId)
    LikeEntity-->>UserEngagementService: likeCreated
    UserEngagementService-->>Server: postLiked
    Server-->>User: postLiked

```
	2. Comment on Post 
```merm
sequenceDiagram
    actor User
    participant Server
    participant UserEngagementService
    participant LikeEntity
    participant CommentEntity
    participant UserEntity
    participant PostEntity

    User->>Server: POST /posts/{postId}/comments (userId, commentData)
    Server->>UserEngagementService: commentOnPost(userId, postId, commentData)
    UserEngagementService->>UserEntity: getUserById(userId)
    UserEntity-->>UserEngagementService: userFound
    UserEngagementService->>PostEntity: getPostById(postId)
    PostEntity-->>UserEngagementService: postFound
    UserEngagementService->>CommentEntity: createComment(userId, postId, commentData)
    CommentEntity-->>UserEngagementService: commentCreated
    UserEngagementService-->>Server: commentAdded
    Server-->>User: commentAdded

```
	3. Follow The Auther
```merm
sequenceDiagram
    actor User
    participant Server
    participant UserEngagementService
    participant LikeEntity
    participant CommentEntity
    participant UserEntity

    User->>Server: POST /users/{authorId}/followers (userId)
    Server->>UserEngagementService: followAuthor(userId, authorId)
    UserEngagementService->>UserEntity: getUserById(userId)
    UserEntity-->>UserEngagementService: userFound
    UserEngagementService->>UserEntity: getUserById(authorId)
    UserEntity-->>UserEngagementService: authorFound
    UserEngagementService->>UserEntity: followUser(userId, authorId)
    UserEntity-->>UserEngagementService: followingCreated
    UserEngagementService-->>Server: authorFollowed
    Server-->>User: authorFollowed
```

4. **Content Discovery**
1. Get Post By The Query
```merm
sequenceDiagram
    actor User
    participant Server
    participant ContentDiscoveryService
    participant PostEntity

    User->>Server: GET /posts?query={query}&filters={filters}
    Server->>ContentDiscoveryService: searchPosts(query, filters)
    ContentDiscoveryService->>PostEntity: getPosts(query, filters)
    PostEntity-->>ContentDiscoveryService: postsFound
    ContentDiscoveryService-->>Server: searchResults
    Server-->>User: searchResults
```
2. Get Feature Posts
```merm
sequenceDiagram
    actor User
    participant Server
    participant ContentDiscoveryService
    participant PostEntity


    User->>Server: GET /posts/featured
    Server->>ContentDiscoveryService: getFeaturedPosts()
    ContentDiscoveryService->>PostEntity: getFeaturedPosts()
    PostEntity-->>ContentDiscoveryService: featuredPostsFound
    ContentDiscoveryService-->>Server: featuredPosts
    Server-->>User: featuredPosts
    
```
3. Get Recommended Posts
```merm
sequenceDiagram
    actor User
    participant Server
    participant ContentDiscoveryService
    participant PostEntity
    participant TagEntity
    participant CategoryEntity


    User->>Server: GET /users/{userId}/recommended-posts
    Server->>ContentDiscoveryService: getRecommendedPosts(userId)
    ContentDiscoveryService->>UserEntity: getUserById(userId)
    UserEntity-->>ContentDiscoveryService: userFound
    ContentDiscoveryService->>PostEntity: getRecommendedPosts(userId)
    PostEntity-->>ContentDiscoveryService: recommendedPostsFound
    ContentDiscoveryService-->>Server: recommendedPosts
    Server->>User:Responed

```
4. Get Posts By Post Tag
```merm
sequenceDiagram
    actor User
    participant Server
    participant ContentDiscoveryService
    participant PostEntity
    participant TagEntity

    User->>Server: GET /posts/tags/{tagId}
    Server->>ContentDiscoveryService: searchByPostTag(tagId)
    ContentDiscoveryService->>TagEntity: getTagById(tagId)
    TagEntity-->>ContentDiscoveryService: tagFound
    ContentDiscoveryService->>PostEntity: getPostsByTag(tagId)
    PostEntity-->>ContentDiscoveryService: postsFoundByTag
    ContentDiscoveryService-->>Server: searchResults
    Server-->>User: searchResults
```
5. Get Posts By Post Category
```merm
sequenceDiagram
    actor User
    participant Server
    participant ContentDiscoveryService
    participant PostEntity
    participant TagEntity
    participant CategoryEntity

    User->>Server: GET /posts/categories/{categoryId}
    Server->>ContentDiscoveryService: searchByPostCategory(categoryId)
    ContentDiscoveryService->>CategoryEntity: getCategoryById(categoryId)
    CategoryEntity-->>ContentDiscoveryService: categoryFound
    ContentDiscoveryService->>PostEntity: getPostsByCategory(categoryId)
    PostEntity-->>ContentDiscoveryService: postsFoundByCategory
    ContentDiscoveryService-->>Server: searchResults
    Server-->>User: searchResults
```

5. **Analytics**
```merm
sequenceDiagram
    participant User
    participant Server
    participant AnalyticsService
    participant User
    participant PostEntity
    participant UserEntity

	rect rgb(200, 200, 255, 0.5)
    User->>Server: GET /posts/{postId}/analytics (userId)
    Server->>AnalyticsService: getPostAnalytics(userId, postId)
    AnalyticsService->>UserEntity: getUserById(userId)
    UserEntity-->>AnalyticsService: userFound
    AnalyticsService->>PostEntity: getPostById(postId)
    PostEntity-->>AnalyticsService: postFound
    AnalyticsService->>PostEntity: getPostAnalytics(postId)
    PostEntity-->>AnalyticsService: postAnalytics
    AnalyticsService-->>Server: postAnalytics
    Server-->>User: postAnalytics
    end

	rect rgb(244, 200, 255, 0.5)
    User->>Server: POST /analytics/tools (toolName, config)
    Server->>AnalyticsService: integrateTool(toolName, config)
    AnalyticsService-->>Server: toolIntegrated
    Server-->>User: toolIntegrated
    end

	rect rgb(230, 230, 255, 0.5)
    User->>Server: GET /analytics/google
    Server->>AnalyticsService: googleAnalytics()
    AnalyticsService->>PostEntity: getGoogleAnalyticsData()
    PostEntity-->>AnalyticsService: googleAnalyticsData
    AnalyticsService-->>Server: googleAnalyticsData
    Server-->>User: googleAnalyticsData
    end
```

