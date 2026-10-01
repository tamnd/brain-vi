---
title: "CF 104857F - Bóng Bay Nhiều Màu Sắc"
description: "Chúng ta được cung cấp một chuỗi các màu bóng, trong đó mỗi quả bóng có một màu được biểu thị bằng một chuỗi chữ thường ngắn. Nhiệm vụ là xác định xem có tồn tại một màu xuất hiện đúng hơn một nửa tổng số quả bóng bay hay không. Nếu màu đó tồn tại, chúng tôi sẽ xuất nó."
date: "2026-06-28T10:55:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104857
codeforces_index: "F"
codeforces_contest_name: "The 2023 ICPC Asia Hefei Regional Contest (The 2nd Universal Cup. Stage 12: Hefei)"
rating: 0
weight: 104857
solve_time_s: 36
verified: true
draft: false
---

[CF 104857F - Bóng bay nhiều màu sắc](https://codeforces.com/problemset/problem/104857/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 36s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các màu bóng, trong đó mỗi quả bóng có một màu được biểu thị bằng một chuỗi chữ thường ngắn. Nhiệm vụ là xác định xem có tồn tại một màu xuất hiện đúng hơn một nửa tổng số quả bóng bay hay không. Nếu màu đó tồn tại, chúng tôi sẽ xuất nó. Nếu không, chúng tôi xuất ra chuỗi lỗi “uh-oh”. 

Cấu trúc về cơ bản là về sự thống trị trong nhiều chuỗi. Chúng tôi không được yêu cầu liệt kê tần số hoặc tính toán bất kỳ điều gì khác ngoài việc xác định liệu phần tử đa số nghiêm ngặt có tồn tại hay không và nếu có thì trả về nó. 

Kích thước đầu vào lên tới 100000 chuỗi, mỗi chuỗi có độ dài tối đa 10. Điều này ngay lập tức gợi ý rằng bất kỳ thuật toán nào so sánh từng cặp chuỗi hoặc quét liên tục toàn bộ danh sách để tìm từng ứng cử viên sẽ quá chậm. Một giải pháp đếm tần số theo thời gian tuyến tính bằng cách sử dụng hàm băm là phù hợp, vì tổng số thao tác sẽ theo thứ tự 100000 lần chèn và tra cứu, nằm trong giới hạn thông thường. 

Trường hợp khó phát hiện khi không có màu nào vượt quá ngưỡng 50%. Ví dụ: nếu đầu vào có ba màu riêng biệt như đỏ, xanh, vàng, mỗi màu xuất hiện một lần thì không có màu đầu ra nào đủ tiêu chuẩn và câu trả lời phải là “uh-oh”. Một trường hợp khác là khi chính xác một nửa số quả bóng bay có một màu. Ví dụ: trong n = 4, nếu một màu xuất hiện đúng 2 lần thì màu đó không đáp ứng được yêu cầu khắt khe “hơn một nửa” nên không được chấp nhận. 

Một trường hợp quan trọng khác là khi có nhiều màu nhưng một màu hầu như không vượt qua ngưỡng, chẳng hạn như 5 quả bóng bay có số màu đỏ = 3, xanh lục = 2. Chỉ có màu chủ đạo là hợp lệ, mặc dù các màu khác có thể xuất hiện thường xuyên. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là đếm số lần xuất hiện của từng màu bằng cách quét toàn bộ danh sách và đối với mỗi màu riêng biệt, tính toán lại tần số của nó bằng một lần quét khác. Điều này sẽ yêu cầu O(n) hoạt động trên mỗi màu, dẫn đến O(n^2) trong trường hợp xấu nhất khi tất cả các màu đều khác biệt. Với n lên tới 100000, tốc độ này quá chậm. 

Quan sát quan trọng là chúng ta chỉ quan tâm đến tần số được tổng hợp trên các chuỗi giống hệt nhau. Khi chúng tôi duy trì bản đồ tần số trong khi đọc đầu vào, chúng tôi có thể xác định tần số tối đa trong một lần truyền. Điều này làm giảm vấn đề về việc đếm và sau đó kiểm tra xem số lượng tối đa có vượt quá n/2 hay không. 

Một góc nhìn khác cho rằng đây là một vấn đề về phần tử đa số cổ điển trên chuỗi. Mặc dù các thuật toán như phiếu bầu đa số của Boyer-Moore tồn tại nhưng chúng không cần thiết ở đây vì bản đồ băm cung cấp giải pháp đơn giản hơn và hiệu quả tương đương với các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Đếm lực lượng vũ phu trên mỗi màu | O(n^2) | O(1) | Quá chậm | 
| Bản đồ tần số | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một từ điển lưu trữ số lần mỗi màu xuất hiện trong khi đọc dữ liệu đầu vào. 

1. Khởi tạo bản đồ băm trống để lưu trữ tần số màu. Điều này cho phép cập nhật và truy vấn theo thời gian liên tục trên mỗi chuỗi. 
2. Đọc từng màu bóng bay một và tăng số lượng của nó trên bản đồ. Điều này đảm bảo rằng khi kết thúc quá trình xử lý đầu vào, chúng tôi có thông tin tần số hoàn chỉnh mà không cần phải chuyển thêm. 
3. Theo dõi màu sắc với tần suất cao nhất khi chúng tôi cập nhật bản đồ. Điều này tránh việc quét bản đồ sau đó và giữ cho giải pháp tuyến tính hoàn toàn. 
4. Sau khi xử lý tất cả các màu, so sánh tần số tối đa với n chia cho 2. Nếu nó lớn hơn thực sự, hãy xuất ra màu tương ứng. 
5. Nếu không, xuất ra “uh-oh”. 

Lý do chúng ta sử dụng phép so sánh chặt chẽ là rất quan trọng: đẳng thức với một nửa không thỏa mãn điều kiện của bài toán. 

### Tại sao nó hoạt động

Tại bất kỳ thời điểm nào trong quá trình xử lý, bản đồ tần số phản ánh chính xác số lần xuất hiện cho đến nay của mỗi màu. Do đó, màu có tần số tối đa ở cuối sẽ là màu tối đa hóa toàn cục trên toàn bộ tập dữ liệu. Vì điều kiện về tính đúng đắn chỉ phụ thuộc vào việc một giá trị có vượt quá n/2 hay không, nên việc xác định tần số tối đa là đủ để quyết định câu trả lời. Không có thuộc tính cấu trúc nào khác của chuỗi quan trọng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    freq = {}
    best_color = ""
    best_count = 0

    for _ in range(n):
        c = input().strip()
        freq[c] = freq.get(c, 0) + 1

        if freq[c] > best_count:
            best_count = freq[c]
            best_color = c

    if best_count > n // 2:
        print(best_color)
    else:
        print("uh-oh")

if __name__ == "__main__":
    solve()
```Giải pháp duy trì một từ điển`freq`để đếm số lần xuất hiện. Biến`best_count`theo dõi tần số lớn nhất được thấy cho đến nay và`best_color`nhớ màu nào đã đạt được nó. Điều này tránh việc phải duyệt lại từ điển lần thứ hai. 

Một sai lầm phổ biến là sử dụng`>= n // 2`thay vì`> n // 2`, chấp nhận sai các màu xuất hiện đúng một nửa thời gian. Một cạm bẫy tiềm ẩn khác là quên loại bỏ các ký tự dòng mới khi đọc chuỗi, điều này sẽ khiến các màu giống hệt nhau được coi là các phím khác nhau. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
red
green
red
red
blue
```| Bước | Màu sắc | tần số (đỏ) | tần số(xanh) | tần số (màu xanh) | màu_tốt nhất | tốt nhất | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | đỏ | 1 | 0 | 0 | đỏ | 1 | 
| 2 | màu xanh lá cây | 1 | 1 | 0 | đỏ | 1 | 
| 3 | đỏ | 2 | 1 | 0 | đỏ | 2 | 
| 4 | đỏ | 3 | 1 | 0 | đỏ | 3 | 
| 5 | màu xanh | 3 | 1 | 1 | đỏ | 3 | 

Tần số tối đa cuối cùng là 3 và n/2 là 2,5, do đó màu đỏ hoàn toàn lớn hơn một nửa và được in. 

### Ví dụ 2 

đầu vào:```
3
red
blue
yellow
```| Bước | Màu sắc | tần số (đỏ) | tần số (màu xanh) | tần số(vàng) | màu_tốt nhất | tốt nhất | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | đỏ | 1 | 0 | 0 | đỏ | 1 | 
| 2 | màu xanh | 1 | 1 | 0 | đỏ | 1 | 
| 3 | màu vàng | 1 | 1 | 1 | đỏ | 1 | 

Ở đây tần số tối đa là 1, trong khi n/2 là 1,5, do đó không tồn tại đa số hợp lệ và đầu ra là “uh-oh”. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi bong bóng được xử lý một lần với O(1) thao tác từ điển trung bình | 
| Không gian | O(k) | k màu riêng biệt được lưu trữ trong bản đồ băm, trường hợp xấu nhất k = n | 

Các ràng buộc cho phép lên tới 100000 bong bóng, do đó, quét tuyến tính với hàm băm là hiệu quả thoải mái. Việc sử dụng bộ nhớ cũng an toàn vì mỗi chuỗi đều ngắn và chỉ lưu trữ các màu riêng biệt. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample 1
assert run("""5
red
green
red
red
blue
""") == "red"

# sample 2
assert run("""3
red
blue
yellow
""") == "uh-oh"

# all same
assert run("""4
a
a
a
a
""") == "a"

# exactly half (invalid)
assert run("""4
a
a
b
b
""") == "uh-oh"

# single element
assert run("""1
abc
""") == "abc"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều giống nhau | một | trường hợp thống trị hoàn toàn | 
| chia một nửa | ồ-ồ | điều kiện đa số nghiêm ngặt | 
| phần tử đơn | abc | trường hợp ranh giới tối thiểu | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các màu đều khác biệt. Bản đồ tần số vẫn ghi lại mỗi màu một lần, nhưng không có giá trị nào vượt quá n/2. Đối với đầu vào:```
4
a
b
c
d
```thuật toán theo dõi từng tần số là 1 và vì 1 không lớn hơn 2 nên thuật toán sẽ xuất ra “uh-oh” một cách chính xác. 

Một trường hợp khác là khi đa số tồn tại nhưng lại xuất hiện ở các vị trí không liên tiếp. Ví dụ:```
7
x
y
x
z
x
x
y
```Tần số của x trở thành 4, vượt quá 7/2 = 3,5. Mặc dù các lần xuất hiện rải rác nhưng bản đồ vẫn tổng hợp chính xác và thuật toán đưa ra x.
