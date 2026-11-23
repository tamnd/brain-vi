---
title: "CF 421A - Pasha và chuột đồng"
description: "Chúng ta được cho một hàng táo được đánh số từ 1 đến n. Mỗi quả táo có thể được trao cho chính xác một trong hai chú chuột hamster, Arthur hoặc Alexander. Hạn chế duy nhất là Arthur chỉ được nhận những quả táo anh ấy thích, còn Alexander chỉ được nhận những quả táo anh ấy thích."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation"]
categories: ["algorithms"]
codeforces_contest: 421
codeforces_index: "A"
codeforces_contest_name: "Coder-Strike 2014 - Finals (online edition, Div. 2)"
rating: 800
weight: 421
solve_time_s: 60
verified: true
draft: false
---

[CF 421A - Pasha và Hamster](https://codeforces.com/problemset/problem/421/A) 

**Đánh giá:** 800 
**Tas:** thuật toán xây dựng, triển khai 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hàng táo được đánh số từ 1 đến n. Mỗi quả táo có thể được trao cho chính xác một trong hai chú chuột hamster, Arthur hoặc Alexander. Hạn chế duy nhất là Arthur chỉ được nhận những quả táo anh ấy thích, còn Alexander chỉ được nhận những quả táo anh ấy thích. Một số quả táo có thể được cả hai thích, và chúng có thể thuộc về một trong hai con hamster, nhưng vẫn chỉ có một trong số chúng. 

Nhiệm vụ của chúng ta là chỉ định mỗi quả táo cho đúng một chú hamster đồng thời tôn trọng cả hai danh sách ưu tiên. Đầu vào cung cấp n, sau đó là danh sách các chỉ số mà Arthur thích và danh sách các chỉ số mà Alexander thích. Chúng ta phải xuất ra một chuỗi có độ dài n gồm 1 và 2 cho biết quyền sở hữu. 

Các ràng buộc rất nhỏ: n nhiều nhất là 100. Điều đó ngay lập tức cho chúng ta biết rằng ngay cả các chiến lược gán O(n²) hoặc brute cũng dễ dàng đủ nhanh. Không cần cấu trúc dữ liệu nâng cao hoặc thủ thuật tối ưu hóa. Vấn đề cơ bản là giải quyết sự chồng chéo một cách nhất quán. 

Một sự tinh tế quan trọng là những quả táo không thuộc danh sách nào. Tuyên bố đảm bảo rằng một giải pháp tồn tại, nhưng việc triển khai đơn giản có thể bỏ qua những trường hợp này hoặc không chỉ định chúng một cách xác định. 

Trường hợp tinh vi thứ hai là sự chồng chéo giữa các sở thích. Nếu một quả táo được cả hai chú chuột hamster thích thì việc giao nó một cách tùy tiện vẫn có tác dụng, nhưng nếu cố gắng “cân bằng” hoặc “tối ưu hóa” mà không cẩn thận, chúng ta có thể vô tình bỏ sót một số quả táo không được giao. 

## Phương pháp tiếp cận 

Một quan điểm bạo lực sẽ là thử tất cả các phép gán n quả táo có thể có cho hai con chuột đồng và kiểm tra tính hợp lệ. Mỗi quả táo có 2 lựa chọn nên có 2^n bài tập. Đối với mỗi bài tập, chúng tôi xác minh rằng không có quả táo nào được đưa cho chú chuột hamster không thích nó. Chi phí xác minh là O(n), do đó tổng công việc là O(n·2^n), điều này trở nên không cần thiết ngay cả khi n = 100. 

Cấu trúc của vấn đề khiến cho sức mạnh vũ phu trở nên quá mức cần thiết vì không có sự tương tác ràng buộc toàn cầu nào ngoài khả năng tương thích cá nhân. Mỗi quả táo là độc lập. Điều đó có nghĩa là chúng ta không cần phải tìm kiếm gì cả; chúng ta chỉ cần gán cục bộ cho mỗi quả táo. 

Quan sát quan trọng là mỗi quả táo đều thuộc một trong ba loại: chỉ được Arthur thích, chỉ được Alexander thích hoặc được cả hai thích (hoặc có thể là không). Vì tuyên bố đảm bảo rằng một giải pháp tồn tại nên những quả táo không được cả hai người ưa thích không được tồn tại theo cách có vấn đề hoặc chúng có thể được chỉ định một cách tùy tiện mà không vi phạm tính hợp lệ. Vì vậy, mỗi vị trí có thể được quyết định độc lập. 

Điều này giúp giải quyết vấn đề chỉ bằng một lần quét đơn giản: với mỗi quả táo, hãy gán nó cho bất kỳ chú chuột hamster nào thích nó. Nếu cả hai đều thích thì chọn nhất quán hoặc tùy ý. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n·2^n) | O(n) | Quá chậm | 
| Nhiệm vụ tham lam | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc n, a, b và hai danh sách ưu tiên. 

Chúng tôi sẽ chuyển đổi danh sách ưu tiên thành cấu trúc tra cứu nhanh để có thể kiểm tra tư cách thành viên trong thời gian liên tục. 
2. Xây dựng hai mảng hoặc bộ boolean: một cho những quả táo mà Arthur yêu thích và một cho những quả táo mà Alexander yêu thích. 

Điều này đảm bảo kiểm tra tư cách thành viên O(1) trên mỗi quả táo. 
3. Khởi tạo mảng câu trả lời có độ dài n. 
4. Lặp lại tất cả các quả táo từ 1 đến n. 

Đối với mỗi quả táo, hãy quyết định chủ nhân của nó chỉ dựa trên sở thích. 
5. Nếu quả táo được Arthur thích và không nhất thiết phải bị hạn chế, hãy giao nó cho Arthur (kết quả 1). 

Sự lựa chọn này là tùy ý trong các trường hợp chồng chéo nhưng nhất quán và an toàn. 
6. Nếu không, hãy gán nó cho Alexander (đầu ra 2). 

Vì đầu vào đảm bảo tính khả thi nên mỗi quả táo phải được ít nhất một con hamster thích, vì vậy dự phòng này luôn hợp lệ. 
7. In bài tập kết quả. 

### Tại sao nó hoạt động

Tính đúng đắn xuất phát từ sự độc lập của các quyết định. Mỗi quả táo không có sự phụ thuộc vào bất kỳ quả táo nào khác. Ràng buộc duy nhất là tính hợp lệ cục bộ: một quả táo được giao cho Arthur phải nằm trong bộ của Arthur và tương tự đối với Alexander. Bởi vì chúng tôi chỉ giao táo cho những chú chuột hamster thích chúng nên mọi ràng buộc đều được thỏa mãn. Sự chồng chéo không tạo ra xung đột vì chúng tôi không bao giờ giao một quả táo cho một chú hamster không thích nó. Vì mỗi quả táo được xử lý đúng một lần và được chỉ định ngay lập tức nên không có khả năng xảy ra mâu thuẫn sau này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, a, b = map(int, input().split())

arthur = set(map(int, input().split()))
alex = set(map(int, input().split()))

res = []

for i in range(1, n + 1):
    if i in arthur:
        res.append("1")
    else:
        res.append("2")

print(" ".join(res))
```### Ghi chú thực hiện 

Chúng tôi sử dụng các bộ để kiểm tra tư cách thành viên vì chúng cho kết quả tra cứu O(1) trung bình, giúp giải pháp tuyến tính. 

Quy tắc quyết định ưu tiên Arthur bất cứ khi nào có thể. Điều này là tùy tiện; chọn Alexander trước tiên cũng sẽ có hiệu quả miễn là chúng tôi tôn trọng các ràng buộc về tư cách thành viên. 

Chúng ta in các ký tự cách nhau bằng dấu cách theo yêu cầu của định dạng câu lệnh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 2 3
1 2
2 3 4
```Chúng tôi xây dựng: 

Arthur = {1, 2} 

Alexander = {2, 3, 4} 

Bây giờ chúng tôi xử lý từng quả táo: 

| tôi | ở Arthur | ở Alexander | quyết định | 
| --- | --- | --- | --- | 
| 1 | vâng | không | 1 | 
| 2 | vâng | vâng | 1 | 
| 3 | không | vâng | 2 | 
| 4 | không | vâng | 2 | 

Đầu ra:```
1 1 2 2
```Điều này chứng tỏ rằng sự chồng chéo (Apple 2) được giải quyết một cách an toàn mà không có xung đột. 

### Ví dụ 2 

đầu vào:```
3 1 2
1
2 3
```Arthur = {1} 

Alexander = {2, 3} 

| tôi | ở Arthur | ở Alexander | quyết định | 
| --- | --- | --- | --- | 
| 1 | vâng | không | 1 | 
| 2 | không | vâng | 2 | 
| 3 | không | vâng | 2 | 

Đầu ra:```
1 2 2
```Điều này cho thấy sự tách biệt rõ ràng khi các tùy chọn không trùng nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi quả táo được xử lý một lần với bộ tra cứu O(1) | 
| Không gian | O(n) | Bộ lưu trữ tối đa n phần tử | 

Các ràng buộc n ≤ 100 vượt xa mức cần thiết cho giải pháp này. Ngay cả khi n lớn hơn nhiều (như 2e5), cách tiếp cận tương tự vẫn sẽ tối ưu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from io import StringIO as _StringIO

    n, a, b = map(int, sys.stdin.readline().split())
    arthur = set(map(int, sys.stdin.readline().split()))
    alex = set(map(int, sys.stdin.readline().split()))

    res = []
    for i in range(1, n + 1):
        if i in arthur:
            res.append("1")
        else:
            res.append("2")

    return " ".join(res)

# provided sample
assert run("4 2 3\n1 2\n2 3 4\n") == "1 1 2 2"

# all liked by Arthur only
assert run("3 3 0\n1 2 3\n\n") == "1 1 1"

# all liked by Alexander only
assert run("3 0 3\n\n1 2 3\n") == "2 2 2"

# overlap everywhere
assert run("2 2 2\n1 2\n1 2\n") == "1 1"

# single element
assert run("1 1 1\n1\n1\n") in ["1", "2"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu hỗn hợp | 1 1 2 2 | xử lý chồng chéo | 
| tất cả Arthur | 1 1 1 | trường hợp chuyển nhượng hợp lệ duy nhất | 
| tất cả Alexander | 2 2 2 | tính chính xác dự phòng | 
| chồng chéo hoàn toàn | 1 1 | phá ràng buộc tùy tiện | 
| n=1 trường hợp | 1 hoặc 2 | điều kiện biên |
