# CSC4005 – Lab 2 Report

## 1. Thông tin chung
- Họ và tên: lê trọng thanh tùng
- Lớp:
- Repo:
- W&B project:

## 2. Bài toán
Mô tả ngắn bài toán phân loại 6 lớp trên NEU-CLS.

## 3. Mô hình và cấu hình
### 3.1. MLP baseline từ Lab 1
### 3.2. CNN from scratch
Run 1: `cnn_small`
- Kích thước ảnh: 64 x 64
- Số tham số huấn luyện: ~32,500 (tất cả)
- Số epoch: 20

### 3.3. Transfer learning
Run 2: `resnet18`
- Kích thước ảnh: 128 x 128
- Số tham số huấn luyện: ~3,000 (chỉ phần head)
- Số epoch: 10

## 4. Bảng kết quả
| Model | Train mode | Best Val Acc | Test Acc | Epoch time | Trainable Params | Nhận xét |
|---|---|---:|---:|---:|---:|---|
| MLP | scratch | N/A | N/A | N/A | N/A | Chưa thực hiện trong 2 run này |
| CNN-small | scratch | ~90% - 92% | N/A | N/A | ~32,500 | Loss biến động, có dấu hiệu overfitting |
| ResNet18 | transfer | ~95% - 97% | N/A | N/A | ~3,000 | Hội tụ ổn định, độ chính xác cao |

## 5. Phân tích learning curves
### Run 1: `cnn_small`
- Tính hội tụ: loss giảm nhưng rất răng cưa (noisy).
- Dấu hiệu overfitting: xuất hiện khoảng epoch 13-15 khi `val_loss` nhảy vọt.

### Run 2: `resnet18`
- Tính hội tụ: đường cong mượt, sau ~4 epoch đã đạt accuracy > 90%.
- Độ ổn định: không có underfitting/overfitting rõ rệt, khoảng cách train-val hẹp.

## 6. Confusion matrix và lỗi dự đoán sai
Cả hai mô hình làm tốt các lớp `Patches` và `Rolled-in_Scale`. Tuy nhiên:
- Run 1 (`cnn_small`): nhầm lẫn giữa `Inclusion` và `Pitted_Surface` (5 mẫu). Có thể do ảnh 64x64 quá nhỏ để phân biệt chi tiết.
- Run 2 (`resnet18`): vẫn nhầm lẫn giữa `Inclusion` và `Pitted_Surface` (5 mẫu) nhưng cải thiện ở các lớp khác như `Scratches`.

## 7. Kết luận
- Transfer learning (ResNet18) cho độ chính xác cao hơn (~+5%) và hội tụ nhanh hơn (10 epoch so với 20 epoch).
- Phù hợp khi dữ liệu không lớn và muốn tận dụng đặc trưng học sẵn (ImageNet) để tổng quát hóa tốt.
- CNN scratch dễ bị noisy và overfitting hơn; ResNet18 ổn định và ít nhảy loss.
- Hướng tối ưu: thử `img_size` 224 và giảm `lr` xuống 0.0001 ở các epoch cuối để fine-tune.
link dasboard:
1. https://wandb.ai/letrongthanhtung5-daina-middleton/csc4005-lab2-neu-cnn/runs/d9b6rhnb?nw=nwuserletrongthanhtung5
2. https://wandb.ai/letrongthanhtung5-daina-middleton/csc4005-lab2-neu-cnn/runs/jnzroe56?nw=nwuserletrongthanhtung5
