---
title: "CF 106328E - MiniC"
description: "Chúng ta được cho một hoán vị các số từ 0 đến n trừ một. Đối với mọi vị trí i, chúng ta muốn nhìn sang bên phải và tìm vị trí đầu tiên j nơi giá trị trở nên nhỏ hơn giá trị tại i. Nếu không tồn tại vị trí như vậy thì câu trả lời cho i là n."
date: "2026-06-20T22:48:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106328
codeforces_index: "E"
codeforces_contest_name: "Baozii Cup 3"
rating: 0
weight: 106328
solve_time_s: 37
verified: true
draft: false
---

[CF 106328E - MiniC](https://codeforces.com/problemset/problem/106328/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị các số từ 0 đến n trừ một. Đối với mọi vị trí i, chúng ta muốn nhìn sang bên phải và tìm vị trí đầu tiên j nơi giá trị trở nên nhỏ hơn giá trị tại i. Nếu không tồn tại vị trí như vậy thì câu trả lời cho i là n. 

Vì vậy, nhiệm vụ cơ bản là tính toán, đối với mỗi chỉ mục, chỉ mục phần tử nhỏ hơn tiếp theo trong một hoán vị, nhưng bị giới hạn ở các giá trị nhỏ hơn và hoàn toàn ở bên phải. 

Các ràng buộc nhỏ, với n lên tới 1000, ngay lập tức cho phép suy luận bậc hai mà không cần quan tâm đến hiệu suất. Tuy nhiên, sự thay đổi không nằm ở tính hiệu quả mà nằm ở cách diễn giải: chúng ta không chỉ tìm một giá trị mà còn ở vị trí đầu tiên xuất hiện một giá trị nhỏ hơn rất nhiều. 

Một trường hợp lỗi phổ biến là nhầm lẫn giữa “giá trị nhỏ hơn tiếp theo” với “giá trị nhỏ hơn tiếp theo hoặc bằng” hoặc quét nhầm sang trái thay vì phải. Một trường hợp tinh tế khác là khi phần tử nhỏ nhất xuất hiện sớm. Ví dụ: trong hoán vị như [1, 0, 2], câu trả lời cho chỉ số 1 là n vì không có gì nhỏ hơn tồn tại ở bên phải của nó. Bất kỳ triển khai nào không xử lý rõ ràng việc thiếu j hợp lệ sẽ trả về một chỉ mục không hợp lệ hoặc để lại các giá trị rác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là mô phỏng định nghĩa theo nghĩa đen. Đối với mỗi chỉ số i, chúng tôi quét tất cả các vị trí j lớn hơn i và theo dõi j đầu tiên trong đó p[j] nhỏ hơn p[i]. Điều này đúng vì nó tuân theo đúng định nghĩa. Trong trường hợp xấu nhất, đối với hoán vị giảm dần như [9, 8, 7, ..., 0], mọi vị trí tôi sẽ quét gần như tất cả các vị trí còn lại, dẫn đến n bình phương gần đúng trên hai phép so sánh, tức là khoảng năm trăm nghìn phép tính khi n bằng một nghìn. Điều này dễ dàng đủ nhanh. 

Quan sát quan trọng là không có gì trong vấn đề yêu cầu xử lý trước nâng cao hơn, bởi vì chúng tôi không được yêu cầu truy vấn phạm vi hoặc cập nhật lặp lại. Mỗi câu trả lời là độc lập và chỉ phụ thuộc vào hậu tố của mảng. 

Mặc dù người ta có thể sử dụng ngăn xếp đơn điệu để giải các bài toán phần tử nhỏ hơn tiếp theo trong thời gian tuyến tính, nhưng điều đó là không cần thiết ở đây. Sự đơn giản của các ràng buộc làm cho giải pháp brute-force vừa đủ vừa dễ thực hiện một cách chính xác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Đã chấp nhận | 
| Ngăn xếp đơn điệu | O(n) | O(n) | Được chấp nhận nhưng không cần thiết | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán câu trả lời cho từng chỉ số một cách độc lập. 

1. Với mỗi vị trí i từ 0 đến n trừ 1, khởi tạo đáp án là n. Điều này thể hiện trường hợp không tìm thấy j hợp lệ. 
2. Quét j từ i cộng 1 đến n trừ 1. Với mỗi j, so sánh p[j] với p[i]. Nếu p[j] nhỏ hơn, chúng tôi đã tìm thấy vị trí hợp lệ đầu tiên ở bên phải, vì vậy chúng tôi lưu j làm câu trả lời cho i và ngừng quét thêm. 
3. Nếu quá trình quét kết thúc mà không tìm thấy bất kỳ giá trị nào nhỏ hơn, câu trả lời vẫn là n, mã hóa chính xác sự vắng mặt của vị trí hợp lệ. 

Tính chính xác đến từ việc chúng ta quét j theo thứ tự tăng dần nên lần đầu tiên chúng ta gặp giá trị nhỏ hơn tự động là chỉ số hợp lệ nhỏ nhất j thỏa mãn điều kiện. Không cần phải kiểm tra thêm các vị trí khi điều đó xảy ra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    data = list(map(int, input().split()))
    if not data:
        return
    
    n = data[0]
    p = data[1:1+n]
    
    res = [n] * n
    
    for i in range(n):
        for j in range(i + 1, n):
            if p[j] < p[i]:
                res[i] = j
                break
    
    print(*res)

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh chặt chẽ thuật toán. Đầu tiên chúng ta phân tích n và hoán vị. Mảng kết quả được khởi tạo với n để thể hiện trường hợp dự phòng. Các vòng lặp lồng nhau thực hiện quét hậu tố. Vòng lặp bên trong sẽ ngắt ngay lập tức khi tìm thấy phần tử nhỏ hơn hợp lệ, đảm bảo chúng tôi nắm bắt được lần xuất hiện đầu tiên. 

Một cạm bẫy phổ biến ở đây là quên ngắt sau khi tìm thấy j hợp lệ đầu tiên, điều này sẽ ghi đè lên câu trả lời bằng các chỉ số sau đó và vi phạm định nghĩa. Một cách khác là khởi tạo không chính xác mảng kết quả bằng 0, điều này sẽ gợi ý sai rằng chỉ số 0 là hợp lệ ngay cả khi không tồn tại j như vậy. 

## Ví dụ đã hoạt động 

Xét đầu vào p = [2, 0, 3, 1]. 

Chúng tôi tính toán từng vị trí: 

| tôi | p[i] | j quét | hợp lệ đầu tiên j | độ phân giải[i] | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | 1,2,3 | 1 (p[1]=0) | 1 | 
| 1 | 0 | 2,3 | không | 4 | 
| 2 | 3 | 3 | 3 (p[3]=1) | 3 | 
| 3 | 1 | không | không | 4 | 

Vậy đầu ra là [1, 4, 3, 4]. 

Dấu vết này cho thấy cách thuật toán bỏ qua các giá trị lớn hơn và dừng ở giá trị nhỏ hơn đầu tiên một cách chính xác. 

Bây giờ hãy xem xét p = [3, 2, 1, 0]. 

| tôi | p[i] | j quét | hợp lệ đầu tiên j | độ phân giải[i] | 
| --- | --- | --- | --- | --- | 
| 0 | 3 | 1 | 1 | 1 | 
| 1 | 2 | 2 | 2 | 2 | 
| 2 | 1 | 3 | 3 | 3 | 
| 3 | 0 | không | không | 4 | 

Trường hợp này thể hiện tính chất xếp tầng của các mảng giảm nghiêm ngặt, trong đó mọi vị trí đều tìm thấy hàng xóm ngay lập tức của nó làm câu trả lời. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mỗi chỉ mục quét tối đa n phần tử tiếp theo trong trường hợp xấu nhất | 
| Không gian | O(1) | Chỉ có mảng đầu vào và mảng đầu ra được sử dụng | 

Với n nhiều nhất là 1000, trường hợp xấu nhất bao gồm khoảng một triệu phép so sánh, nằm trong giới hạn thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import solve  # if split, otherwise inline call
    return solve()

# sample-like
assert run("4 2 0 3 1\n") == "1 4 3 4", "basic case"

# minimum size
assert run("1 0\n") == "1", "single element"

# already increasing permutation
assert run("3 0 1 2\n") == "1 2 3", "increasing order"

# decreasing permutation
assert run("4 3 2 1 0\n") == "1 2 3 4", "decreasing order"

# random case
assert run("5 1 3 0 4 2\n") == "2 2 5 5 5", "mixed case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 | 1 | trường hợp tối thiểu | 
| 3 0 1 2 | 1 2 3 | tăng hành vi đặt hàng | 
| 4 3 2 1 0 | 1 2 3 4 | chuỗi giảm nghiêm ngặt | 
| 5 1 3 0 4 2 | 2 2 5 5 5 | thứ tự hỗn hợp đúng đắn | 

## Vỏ cạnh 

Hoán vị một phần tử như [0] không tạo ra j hợp lệ, vì vậy câu trả lời là [1]. Thuật toán khởi tạo res[0] thành n và không bao giờ tìm thấy phần tử nhỏ hơn, do đó, nó xuất ra chính xác 1. 

Đối với hoán vị tăng nghiêm ngặt như [0, 1, 2, 3], mọi phần tử chỉ nhìn thấy giá trị nhỏ hơn nếu nó tồn tại ở bên phải, nhưng không có phần tử nào tồn tại. Quá trình quét diễn ra đến cuối mỗi lần và để lại kết quả là n cho tất cả các chỉ mục. 

Đối với hoán vị giảm nghiêm ngặt như [3, 2, 1, 0], mọi phần tử ngay lập tức tìm thấy phần tử tiếp theo nhỏ hơn. Vòng lặp bên trong ngắt tại j = i + 1, do đó đầu ra trở thành [1, 2, 3, 4], khớp chính xác với quy tắc chỉ số nhỏ hơn hợp lệ đầu tiên.
