---
title: "CF 1611B - Thành phần nhóm: Lập trình viên và nhà toán học"
description: "Chúng tôi có hai nhóm sinh viên: lập trình viên và nhà toán học. Từ những học sinh này, chúng tôi muốn thành lập càng nhiều đội càng tốt, mỗi đội có đúng 4 học sinh."
date: "2026-06-10T07:02:20+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "constructive-algorithms", "math"]
categories: ["algorithms"]
codeforces_contest: 1611
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 756 (Div. 3)"
rating: 800
weight: 1611
solve_time_s: 67
verified: true
draft: false
---

[CF 1611B - Thành phần nhóm: Lập trình viên và nhà toán học](https://codeforces.com/problemset/problem/1611/B) 

**Đánh giá:** 800 
**Tags:** tìm kiếm nhị phân, thuật toán xây dựng, toán học 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có hai nhóm sinh viên: lập trình viên và nhà toán học. Từ những học sinh này, chúng tôi muốn thành lập càng nhiều đội càng tốt, mỗi đội có đúng 4 học sinh. 

Có một ràng buộc về cấu trúc đối với các nhóm hợp lệ: một nhóm chỉ được chấp nhận nếu nó có ít nhất một lập trình viên và ít nhất một nhà toán học. Điều này ngay lập tức cấm các đội “thuần túy” được tạo thành hoàn toàn từ một loại, mặc dù họ vẫn đáp ứng yêu cầu về quy mô. 

Nhiệm vụ là tối đa hóa số lượng các đội hợp lệ rời rạc. Mỗi học sinh chỉ được sử dụng tối đa một lần, vì vậy khi một người được chỉ định vào một nhóm, họ không thể đóng góp cho nhóm khác. 

Kích thước đầu vào lớn, có tối đa 10^4 trường hợp thử nghiệm và giá trị của a và b lên tới 10^9. Điều này loại trừ mọi mô phỏng lặp lại trên các nhóm hoặc cá nhân có thể có. Bất kỳ cách tiếp cận nào cũng phải giảm từng trường hợp thử nghiệm thành lý luận O(1). 

Một trường hợp đơn giản nhưng quan trọng phát sinh khi một nhóm rất nhỏ. Ví dụ: nếu a = 10 và b = 1, chúng ta thậm chí không thể thành lập một nhóm hợp lệ duy nhất yêu cầu cả hai loại về số lượng, mặc dù tổng số học sinh là 11. Một cách tiếp cận bất cẩn như sàn((a + b) / 4) sẽ đưa ra không chính xác 2, bỏ qua ràng buộc thành phần. 

Một trường hợp khác là khi một nhóm cực kỳ chiếm ưu thế. Nếu a = 100 và b = 1, chúng ta vẫn không thể thành lập nhiều hơn một đội vì sau khi sử dụng nhà toán học duy nhất, không thể thành lập thêm đội hợp lệ nào nữa. 

Khó khăn chính là cân bằng đồng thời hai ràng buộc: quy mô nhóm là cố định, nhưng thành phần yêu cầu ít nhất một hạn chế từ mỗi nhóm. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề này là thử xây dựng từng nhóm một. Ở mỗi bước, chúng tôi chọn 4 học sinh và kiểm tra xem nhóm có chứa cả hai loại hay không. Nếu không, chúng tôi sẽ loại bỏ lựa chọn đó và thử nhóm khác. Điều này nhanh chóng trở nên không khả thi vì ngay cả việc quyết định chọn tập hợp con nào trong số 4 cũng là tổ hợp và với tối đa 10^9 học sinh cho mỗi danh mục, không gian trạng thái là rất lớn. 

Một lực lượng vũ phu có cấu trúc chặt chẽ hơn là để mô phỏng việc xây dựng tham lam: liên tục lấy 1 lập trình viên và 3 nhà toán học hoặc 2 và 2 hoặc tất cả các phần tách hợp lệ, tùy theo điều kiện nào có thể, cho đến khi chúng ta không thể thành lập một nhóm. Điều này vẫn có nguy cơ khám phá nhiều kết hợp trên mỗi bước và suy biến thành thời gian tuyến tính về số lượng học sinh, quá chậm với tối đa 10^4 trường hợp thử nghiệm và có khả năng là 10^9 phần tử cho mỗi trường hợp thử nghiệm. 

Quan sát quan trọng là mỗi đội có chính xác 4 sinh viên và mỗi đội phải có ít nhất một lập trình viên và ít nhất một nhà toán học. Điều này có nghĩa là mỗi đội giảm cả a và b ít nhất 1. Vì vậy số đội bị giới hạn ở trên bởi min(a, b). Đồng thời, tổng số học sinh giới hạn các đội bằng (a + b) // 4. 

Hai ràng buộc này độc lập và cả hai phải giữ đồng thời. Do đó, số lượng đội tối đa bị giới hạn bởi bất kỳ ràng buộc nào chặt chẽ hơn: hoặc chúng tôi hết một hạng mục hoặc chúng tôi hết tổng số học sinh theo nhóm bốn. 

Vấn đề giảm xuống còn việc lựa chọn mức tối thiểu của hai nút thắt này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(a + b) mỗi bài kiểm tra | O(1) | Quá chậm | 
| Công thức trực tiếp | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Với mỗi test case, hãy đọc giá trị a và b. Chúng đại diện cho số lượng sinh viên của mỗi loại có sẵn. 
2. Tính số lượng đội tối đa được giới hạn hoàn toàn bởi tổng số học sinh là (a + b) // 4. Điều này giả định rằng mỗi đội luôn sử dụng chính xác bốn người, bất kể phân bổ loại. 
3. Tính số lượng đội tối đa bị giới hạn bởi nhóm nhỏ hơn là min(a, b). Mỗi đội phải có ít nhất một học sinh từ mỗi hạng, vì vậy sau khi thành lập k đội, cả a và b vẫn phải giảm ít nhất k. 
4. Câu trả lời cuối cùng là giá trị nhỏ hơn trong hai giá trị này, vì cả hai ràng buộc phải được thỏa mãn đồng thời. 

### Tại sao nó hoạt động 

Mỗi đội hợp lệ sử dụng ít nhất một lập trình viên và ít nhất một nhà toán học, vì vậy sau khi thành lập k đội, chúng ta phải có k  a và k  b, tức là k  min(a, b). Độc lập, mỗi đội có đúng 4 học sinh, vì vậy k đội yêu cầu 4k ≤ a + b, ngụ ý k ≤ (a + b) // 4. Bất kỳ cách xây dựng hợp lệ nào cũng phải tuân theo cả hai bất đẳng thức, vì vậy giá trị cực đại thực sự là giới hạn chặt chẽ nhất giữa chúng. Bởi vì cả hai giới hạn đều có thể đạt được khi kết hợp (bằng cách trộn các thành phần nhóm một cách thích hợp bất cứ khi nào cả hai bên có đủ nguồn cung), mức tối thiểu của cả hai là câu trả lời chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        a, b = map(int, input().split())
        print(min(min(a, b), (a + b) // 4))

if __name__ == "__main__":
    solve()
```Mã xử lý từng trường hợp thử nghiệm một cách độc lập trong thời gian không đổi. Biểu thức min(a, b) thể hiện yêu cầu mỗi nhóm phải bao gồm cả hai loại. Biểu thức (a + b) // 4 thể hiện giới hạn kích thước cố định của mỗi đội. 

Một điểm tinh tế là chúng tôi chưa bao giờ xây dựng nhóm một cách rõ ràng. Lý do hoàn toàn dựa vào những hạn chế về nguồn lực toàn cầu, giúp tránh mọi cạm bẫy về trật tự hoặc tham lam. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp để xem công thức hoạt động như thế nào. 

### Ví dụ 1: a = 5, b = 5 

| Bước | một | b | phút(a,b) | (a+b)//4 | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 5 | 5 | 5 | 2 | 2 | 

Ở đây tổng số nhóm cho phép tối đa 2 đội vì 10 học sinh tạo thành hai nhóm 4 người và còn lại 2 đội. Mặc dù cả hai loại đều rất phong phú nhưng hạn chế về tổng kích thước vẫn chiếm ưu thế. 

Điều này cho thấy rằng việc có các nhóm cân bằng không tự động tăng câu trả lời vượt quá giới hạn đóng gói do quy mô nhóm 4 áp đặt. 

### Ví dụ 2: a = 10, b = 1 

| Bước | một | b | phút(a,b) | (a+b)//4 | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 10 | 1 | 1 | 2 | 1 | 

Ở đây các nhà toán học là nút cổ chai. Mặc dù có đủ tổng số học sinh cho 2 đội nhưng chúng ta chỉ có một nhà toán học nên chỉ có thể thành lập một đội hợp lệ. 

Điều này xác nhận rằng ràng buộc về thành phần không phụ thuộc vào tổng công suất và có thể chi phối câu trả lời. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm được xử lý với số lượng phép tính số học không đổi | 
| Không gian | O(1) | Không có cấu trúc phụ trợ nào được sử dụng ngoài các biến đầu vào | 

Giải pháp dễ dàng nằm trong giới hạn vì ngay cả đối với 10^4 trường hợp thử nghiệm, tổng công việc là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        a, b = map(int, input().split())
        out.append(str(min(min(a, b), (a + b) // 4)))
    return "\n".join(out)

# provided samples
assert run("""6
5 5
10 1
2 3
0 0
17 2
1000000000 1000000000
""") == """2
1
1
0
2
500000000"""

# custom cases
assert run("""3
1 1
4 0
3 3
""") == """0
0
1"""

assert run("""2
8 1
7 7
""") == """1
3"""

assert run("""1
0 10
""") == """0"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 / 4 0 / 3 3 | 0/0/1 | Các trường hợp mất cân bằng tối thiểu và cực đoan | 
| 8 1 / 7 7 | 1/3 | Nút thắt cổ chai và tăng trưởng cân bằng | 
| 0 10 | 0 | Trường hợp cạnh không có sẵn | 

## Vỏ cạnh 

Với a = 0 và b = 10, công thức cho min(min(0, 10), 10 // 4) = 0. Thuật toán trả về chính xác 0 vì không nhóm nào có thể đáp ứng yêu cầu có ít nhất một lập trình viên. 

Với a = 4 và b = 0, chúng ta lại nhận được 0. Mặc dù tổng kích thước chính xác là 4, nhưng việc thiếu loại thứ hai sẽ ngăn cản việc hình thành bất kỳ đội hợp lệ nào, cho thấy rằng chỉ riêng tổng số chia hết là không đủ. 

Đối với các trường hợp mất cân bằng cao như a = 1000000000 và b = 1, kết quả sẽ là 1. Phép tính (a + b) // 4 sẽ gợi ý một số lớn, nhưng ràng buộc min(a, b) giới hạn chính xác ở mức 1, phản ánh rằng chỉ có thể có một nhóm hỗn hợp hợp lệ trước khi hết tính đa dạng cần thiết.
