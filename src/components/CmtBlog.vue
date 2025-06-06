<template>
    <div class="container mt-5 text-center">
        <h3 class="text-3xl font-bold mb-4 text-primary">Bình luận bài viết</h3>
        <div class="row">
            <div class="col-md-8 mx-auto" v-for="(item, index) in items" :key="index">
                <div class="card shadow mb-4">
                    <img :src="item.image" class="card-img-top" alt="Hình ảnh" style="height: 300px; object-fit: cover;">
                    <div class="card-body d-flex flex-column">
                        <h3 class="card-title text-primary">{{ item.title }}</h3>
                        <p class="card-text flex-grow-1 text-start">{{ item.content }}</p>
                    </div>
                </div>
                
                <!-- Form bình luận -->
                <div class="card shadow mb-4">
                    <div class="card-body">
                        <h5 class="card-title text-primary mb-3">Để lại bình luận</h5>
                        <form @submit.prevent="submitComment">
                            <div class="mb-3">
                                <label for="commentName" class="form-label">Tên của bạn:</label>
                                <input 
                                    type="text" 
                                    id="commentName" 
                                    class="form-control" 
                                    v-model="commentName"
                                    placeholder="Nhập tên của bạn"
                                    required
                                >
                            </div>
                            <div class="mb-3">
                                <label for="commentText" class="form-label">Bình luận:</label>
                                <textarea 
                                    id="commentText" 
                                    class="form-control" 
                                    rows="4"
                                    v-model="commentText"
                                    placeholder="Nhập bình luận của bạn"
                                    required
                                ></textarea>
                            </div>
                            <button type="submit" class="btn btn-success">
                                <i class="fas fa-paper-plane me-2"></i>Gửi bình luận
                            </button>
                        </form>
                    </div>
                </div>

                <!-- Danh sách bình luận -->
                <div v-if="comments.length" class="card shadow">
                    <div class="card-body">
                        <h5 class="card-title text-primary mb-3">
                            <i class="fas fa-comments me-2"></i>
                            Danh sách bình luận ({{ comments.length }})
                        </h5>
                        <div class="comment-list">
                            <div 
                                v-for="(comment, commentIndex) in comments" 
                                :key="commentIndex"
                                class="comment-item p-3 mb-3 border rounded bg-light"
                            >
                                <div class="d-flex justify-content-between align-items-start">
                                    <div class="flex-grow-1">
                                        <h6 class="mb-1 text-primary">
                                            <i class="fas fa-user-circle me-2"></i>{{ comment.name }}
                                        </h6>
                                        <p class="mb-2 text-start">{{ comment.text }}</p>
                                        <small class="text-muted">
                                            <i class="fas fa-clock me-1"></i>{{ comment.timestamp }}
                                        </small>
                                    </div>
                                    <button 
                                        @click="deleteComment(commentIndex)"
                                        class="btn btn-outline-danger btn-sm ms-2"
                                        title="Xóa bình luận"
                                    >
                                        <i class="fas fa-trash"></i>
                                    </button>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Thông báo khi chưa có bình luận -->
                <div v-else class="card shadow">
                    <div class="card-body text-center py-4">
                        <i class="fas fa-comment-slash fa-3x text-muted mb-3"></i>
                        <p class="text-muted">Chưa có bình luận nào. Hãy là người đầu tiên bình luận!</p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref } from 'vue'
import img1 from '../assets/Images/rau.jpg'

const items = [
    { 
        title: '8 loại rau củ quả giàu canxi', 
        content: 'Canxi là khoáng chất rất cần thiết cho cơ thể, đặc biệt là xương và răng. Canxi cũng giúp cơ thể duy trì chức năng của các tế bào thần kinh và cơ bắp. Thiếu canxi có thể dẫn đến loãng xương, đau nhức xương khớp và các vấn đề về tim mạch. Các loại rau củ quả giàu canxi bao gồm: cải bó xôi, cải xanh, súp lơ xanh, đậu phụ, hạnh nhân, mè, cá cơm khô và sữa đậu nành.', 
        image: img1 
    },
];

const commentName = ref('')
const commentText = ref('')
const comments = ref([])

const submitComment = () => {
    if (commentName.value.trim() && commentText.value.trim()) {
        const newComment = {
            name: commentName.value.trim(),
            text: commentText.value.trim(),
            timestamp: new Date().toLocaleString('vi-VN', {
                year: 'numeric',
                month: '2-digit',
                day: '2-digit',
                hour: '2-digit',
                minute: '2-digit'
            })
        }
        
        comments.value.unshift(newComment) // Thêm bình luận mới lên đầu
        
        // Reset form
        commentName.value = ''
        commentText.value = ''
        
        // Hiển thị thông báo thành công
        alert('Bình luận đã được gửi thành công!')
    } else {
        alert('Vui lòng nhập đầy đủ tên và nội dung bình luận!')
    }
}

const deleteComment = (index) => {
    if (confirm('Bạn có chắc chắn muốn xóa bình luận này?')) {
        comments.value.splice(index, 1)
    }
}
</script>

<style scoped>
.card-img-top {
    width: 100%;
    object-fit: cover;
    transition: transform 0.3s ease;
}

.card-img-top:hover {
    transform: scale(1.02);
}

.comment-item {
    transition: all 0.3s ease;
    border-left: 4px solid var(--bs-primary) !important;
}

.comment-item:hover {
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    transform: translateY(-2px);
}

.btn {
    transition: all 0.3s ease;
}

.btn:hover {
    transform: translateY(-1px);
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.comment-list {
    max-height: 500px;
    overflow-y: auto;
}

.comment-list::-webkit-scrollbar {
    width: 6px;
}

.comment-list::-webkit-scrollbar-track {
    background: #f1f1f1;
    border-radius: 3px;
}

.comment-list::-webkit-scrollbar-thumb {
    background: var(--bs-primary);
    border-radius: 3px;
}

.comment-list::-webkit-scrollbar-thumb:hover {
    background: #0056b3;
}

.fas {
    color: var(--bs-primary);
}

.text-primary {
    color: var(--bs-primary) !important;
}

@media (max-width: 768px) {
    .container {
        padding: 0 15px;
    }
    
    .col-md-8 {
        padding: 0 10px;
    }
}
</style>