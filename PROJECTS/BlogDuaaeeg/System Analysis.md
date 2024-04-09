[[BlogDuaaeeg Overview]]

References
- [[App Requirement]]
- [[PROJECTS/BlogDuaaeeg/System Design|System Design]]


## Class Diagram 
- Based on #BlogDuaaeeg-ERD structure we designed. We can diagram application services from #BlogDuaaeeg-feat-reqs  that involve with Database by following.

```merm
classDiagram
    class User {
        -userId: int
        -username: string
        -email: string
        -password: string
        -firstName: string
        -lastName: string
        -bio: string
        -profilePicture: string
        +registerUser(userInfo)
        +authenticateUser(credentials)
        +resetPassword(email)
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
    Post "0..*" --o "0..*" PostTag : tagged
    Post "0..*" --o "0..*" PostCategory : categorized
    Tag "1" --o "0..*" PostTag : tagged
    Category "1" --o "0..*" PostCategory : categorized
    Comment "0..*" --o "1" Post : "commented on"
    Like "0..*" --o "1" PublicationPost : "liked"
    Content "1" --o "0..*" PublicationPost : "publishes"
    Post "0..*" --o "0..*" PublicationPost : "published in"
	Post "0..*" --o "0..*" Content : "submits to"

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
