<template>
  <div class="cmt-blog">
    <h3>Bình luận Blog</h3>
    <div class="comment-form">
      <textarea 
        v-model="newComment" 
        placeholder="Viết bình luận của bạn..."
        rows="3"
        class="comment-input"
      ></textarea>
      <button @click="addComment" class="submit-btn">Gửi bình luận</button>
    </div>
    
    <div class="comments-list">
      <div 
        v-for="comment in comments" 
        :key="comment.id"
        class="comment-item"
      >
        <div class="comment-author">{{ comment.author }}</div>
        <div class="comment-content">{{ comment.content }}</div>
        <div class="comment-time">{{ comment.time }}</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CmtBlog',
  data() {
    return {
      newComment: '',
      comments: [
        {
          id: 1,
          author: 'Người dùng 1',
          content: 'Bài viết rất hay và bổ ích!',
          time: '2 giờ trước'
        },
        {
          id: 2,
          author: 'Người dùng 2', 
          content: 'Cảm ơn bạn đã chia sẻ thông tin hữu ích.',
          time: '1 giờ trước'
        }
      ]
    }
  },
  methods: {
    addComment() {
      if (this.newComment.trim()) {
        const newCommentObj = {
          id: this.comments.length + 1,
          author: 'Bạn',
          content: this.newComment,
          time: 'Vừa xong'
        }
        this.comments.unshift(newCommentObj)
        this.newComment = ''
      }
    }
  }
}
</script>

<style scoped>
.cmt-blog {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
}

.comment-form {
  margin-bottom: 30px;
}

.comment-input {
  width: 100%;
  padding: 12px;
  border: 1px solid #ddd;
  border-radius: 8px;
  font-size: 14px;
  resize: vertical;
  margin-bottom: 10px;
}

.submit-btn {
  background-color: #007bff;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 14px;
}

.submit-btn:hover {
  background-color: #0056b3;
}

.comments-list {
  space-y: 15px;
}

.comment-item {
  background-color: #f8f9fa;
  padding: 15px;
  border-radius: 8px;
  margin-bottom: 15px;
  border-left: 4px solid #007bff;
}

.comment-author {
  font-weight: bold;
  color: #333;
  margin-bottom: 5px;
}

.comment-content {
  color: #555;
  line-height: 1.5;
  margin-bottom: 8px;
}

.comment-time {
  font-size: 12px;
  color: #888;
}
</style>