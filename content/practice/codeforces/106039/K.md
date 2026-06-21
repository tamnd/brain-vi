---
title: "CF 106039K - Người Ghét Bánh"
description: "Chúng tôi được cung cấp một bộ nguyên liệu và danh sách các nguyên liệu mà Antonio từ chối sử dụng. Công thức được chia thành nhiều giai đoạn và mỗi giai đoạn chỉ định một tập hợp con các thành phần cần thiết cho bước đó. Khi Antonio chuẩn bị sân khấu, anh ấy chỉ cần bỏ qua mọi thành phần mà anh ấy không thích."
date: "2026-06-20T21:09:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106039
codeforces_index: "K"
codeforces_contest_name: "2025 USP Try-outs"
rating: 0
weight: 106039
solve_time_s: 43
verified: true
draft: false
---

[CF 106039K - Kẻ ghét bánh](https://codeforces.com/problemset/problem/106039/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một bộ nguyên liệu và danh sách các nguyên liệu mà Antonio từ chối sử dụng. Công thức được chia thành nhiều giai đoạn và mỗi giai đoạn chỉ định một tập hợp con các thành phần cần thiết cho bước đó. 

Khi Antonio chuẩn bị sân khấu, anh ấy chỉ cần bỏ qua mọi thành phần mà anh ấy không thích. Điều này có thể làm giảm số lượng thành phần thực sự được sử dụng trong giai đoạn đó. Một công đoạn sẽ không được chấp nhận nếu tỷ lệ nguyên liệu bị thiếu vượt quá 1/3 số nguyên liệu ban đầu trong công đoạn đó. 

Nhiệm vụ là tìm ra giai đoạn sớm nhất mà tình trạng này xảy ra, nghĩa là giai đoạn đầu tiên mà số lượng thành phần bị thiếu lớn hơn một phần ba kích thước của giai đoạn. Nếu không có giai đoạn nào vi phạm điều kiện này, chúng ta sẽ xuất ra âm một. 

Các ràng buộc ngụ ý rằng tổng số thành phần được đề cập trong tất cả các giai đoạn nhiều nhất là 300.000. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào quét liên tục các cấu trúc lớn trên mỗi giai đoạn theo kiểu bậc hai. Có thể chấp nhận quét tuyến tính theo từng giai đoạn đối với các thành phần của chính nó vì mỗi thành phần chỉ xuất hiện một lần trong mỗi giai đoạn và tổng kích thước giữa các giai đoạn là bị giới hạn. 

Trường hợp cạnh tinh tế xuất hiện khi kích thước sân khấu nhỏ. Nếu một giai đoạn có kích thước 1 thì một thành phần bị thiếu đã khiến giai đoạn đó không hợp lệ vì 1 lớn hơn 1/3. Tương tự, đối với cỡ 2, thiếu ít nhất một thành phần là đủ vì 1 > 2/3 là sai nhưng 2 > 2/3 đúng. Vì vậy, trong thực tế, ngưỡng rất nhạy cảm với phép chia số nguyên và phải được xử lý theo bất đẳng thức nghiêm ngặt thay vì làm tròn. 

Một trường hợp khó khăn khác là khi không có thành phần không thích nào xuất hiện ở bất kỳ giai đoạn nào. Đầu ra đúng là trừ một và việc triển khai ngây thơ khởi tạo câu trả lời không chính xác hoặc quên kiểm tra tất cả các giai đoạn có thể vô tình trả về 0 hoặc giá trị chưa được khởi tạo. 

## Phương pháp tiếp cận 

Cách trực tiếp để giải quyết vấn đề là mô phỏng từng giai đoạn một cách độc lập. Đối với một giai đoạn nhất định, chúng tôi quét tất cả các thành phần của nó và đếm xem có bao nhiêu thành phần thuộc về bộ mà Antonio không thích. Sau đó, chúng tôi so sánh số lượng này với một phần ba kích thước sân khấu. Nếu nó lớn hơn, giai đoạn này không hợp lệ. 

Phương pháp brute-force này đã rất gần đến mức tối ưu vì mỗi thành phần được xử lý một lần trong mỗi công đoạn và tổng kích thước của tất cả các công đoạn nhiều nhất là 300.000. Mối quan tâm thực sự duy nhất là cách chúng tôi kiểm tra xem một thành phần có bị ghét hay không. Nếu chúng tôi lưu trữ các thành phần không thích trong một tập hợp băm, thì số lần kiểm tra tư cách thành viên trung bình là O(1), làm cho toàn bộ giải pháp trở nên tuyến tính trong tổng kích thước đầu vào. 

Nhận xét quan trọng là không có sự phụ thuộc lẫn nhau giữa các giai đoạn. Mỗi giai đoạn có thể được đánh giá độc lập chỉ bằng cách sử dụng số lần trùng lặp với tập hợp không thích. Điều này loại bỏ mọi nhu cầu về cấu trúc tiền tố, sắp xếp hoặc tiền xử lý ngoài việc xây dựng tập hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force mỗi lần quét giai đoạn | O(tổng thành phần) | O(B) | Đã chấp nhận | 
| Bộ băm đếm thành viên | O(tổng thành phần) | O(B) | Đã chấp nhận | 

Trong thực tế, cả hai mô tả đều áp dụng cùng một cách triển khai vì cấu trúc đầu vào đã giới hạn tổng công việc. 

## Hướng dẫn thuật toán 

### Thuật toán tối ưu 

1. Đọc bộ thành phần không thích và lưu chúng vào bộ băm. Điều này cho phép kiểm tra thời gian liên tục xem có thành phần nào bị Antonio bỏ qua hay không. 
2. Lặp lại từng giai đoạn theo thứ tự. Đối với mỗi giai đoạn, hãy đọc danh sách các thành phần của nó. 
3. Đối với màn hiện tại, hãy đếm xem có bao nhiêu thành phần của nó xuất hiện trong bộ không thích. Con số này thể hiện số lượng thành phần bị thiếu trong thực tế. 
4. Kiểm tra xem số lượng này có lớn hơn một phần ba kích thước sân khấu hay không. Thay vì sử dụng phép chia dấu phẩy động, hãy so sánh`3 * missing > M`, giúp tránh các vấn đề về độ chính xác và giữ mọi thứ ở dạng số nguyên. 
5. Nếu điều kiện đúng, trả về ngay chỉ số giai đoạn hiện tại làm câu trả lời. 
6. Nếu không có giai đoạn nào thỏa mãn điều kiện sau khi xử lý tất cả chúng, hãy trả về trừ một. 

### Tại sao nó hoạt động 

Mỗi giai đoạn được đánh giá độc lập chỉ dựa trên kích thước giao nhau giữa bộ thành phần của giai đoạn và bộ không thích. Tình trạng chỉ phụ thuộc vào số lượng thành phần bị thiếu chứ không phụ thuộc vào danh tính hay vị trí của chúng. Vì chúng tôi tính toán số lượng này một cách chính xác cho từng giai đoạn và sự bất đẳng thức được kiểm tra mà không gần đúng nên giai đoạn vi phạm đầu tiên được tìm thấy theo đúng thứ tự. Không có giai đoạn nào bị bỏ qua hoặc bị ảnh hưởng bởi người khác nên lỗi sớm nhất luôn được phát hiện chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, A, B = map(int, input().split())
    disliked = set(map(int, input().split()))

    for i in range(1, N + 1):
        data = list(map(int, input().split()))
        M = data[0]
        arr = data[1:]

        missing = 0
        for x in arr:
            if x in disliked:
                missing += 1

        if 3 * missing > M:
            print(i)
            return

    print(-1)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách tải tất cả các thành phần không thích vào bộ băm, đây là bước xử lý trước duy nhất cần thiết. Mỗi giai đoạn sau đó được xử lý tuần tự. dòng`3 * missing > M`là so sánh quan trọng mã hóa “hơn một phần ba” mà không có số học dấu phẩy động. 

Vòng lặp trên các thành phần là an toàn vì tổng số mục thành phần trong tất cả các giai đoạn được giới hạn bởi 300.000, do đó, ngay cả trong trường hợp xấu nhất, vòng lặp bên trong chạy tổng cộng 300.000 lần lặp. 

## Ví dụ đã hoạt động 

### Ví dụ Dấu vết 1 

đầu vào:```
3 6 3
1 3 5
4 1 2 4 6
3 2 3 4
6 6 5 4 3 2 1
```Bộ không thích là`{1, 3, 5}`. 

| Sân khấu | M | Thiếu số lượng | Tình trạng`3*missing > M`| Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 1 | 3 > 4 sai | được | 
| 2 | 3 | 1 | 3 > 3 sai | được | 
| 3 | 6 | 3 | 9 > 6 đúng | thất bại | 

Giai đoạn thứ ba là giai đoạn đầu tiên khi các thành phần bị thiếu vượt quá một phần ba kích thước của giai đoạn. Điều này xác nhận rằng thuật toán trì hoãn việc từ chối một cách chính xác cho đến khi ngưỡng bị vi phạm nghiêm trọng. 

### Ví dụ Dấu vết 2 

đầu vào:```
2 5 2
2 4
1 2 3
4 2 4 1 5
```Bộ không thích là`{2, 4}`. 

| Sân khấu | M | Thiếu số lượng | Tình trạng`3*missing > M`| Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 1 | 3 > 3 sai | được | 
| 2 | 4 | 2 | 6 > 4 đúng | thất bại | 

Ở đây, giai đoạn thứ hai thất bại vì thiếu một nửa thành phần, chứng tỏ việc xử lý đúng các ngưỡng có kích thước chẵn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng thành phần) | Mỗi thành phần được kiểm tra một lần dựa trên bộ băm trong tất cả các giai đoạn | 
| Không gian | O(B) | Lưu trữ cho bộ không thích | 

Tổng công việc là tuyến tính trong tổng của tất cả các kích thước giai đoạn, được giới hạn bởi 300.000. Điều này phù hợp thoải mái trong giới hạn một giây trong Python khi sử dụng kiểm tra tư cách thành viên bộ băm và đầu vào nhanh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    from io import StringIO
    backup_stdout = sys.stdout
    sys.stdout = StringIO()
    solve()
    out = sys.stdout.getvalue().strip()
    sys.stdout = backup_stdout
    return out

# provided sample
assert run("""3 6 3
1 3 5
4 1 2 4 6
3 2 3 4
6 6 5 4 3 2 1
""") == "3"

# all good case
assert run("""2 5 1
3
3 1 2 4
2 1 5
""") == "-1"

# immediate failure
assert run("""1 4 2
1 2
3 1 2 3
""") == "1"

# threshold edge case (exact 1/3 should NOT fail)
assert run("""1 3 1
2
3 1 2 3
""") == "-1"

# maximum missing
assert run("""1 5 3
1 2 3
5 1 2 3 4 5
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | 3 | tiến triển tiêu chuẩn qua các giai đoạn | 
| tất cả đều tốt | -1 | không vi phạm khâu | 
| thất bại ngay lập tức | 1 | kích hoạt giai đoạn đầu tiên | 
| ngưỡng ngưỡng | -1 | bình đẳng không được tính là thất bại | 
| thiếu tối đa | 1 | trường hợp vi phạm đầy đủ | 

## Vỏ cạnh 

Kích thước sân khấu nhỏ có thể khiến tình trạng bệnh phát sinh nhanh chóng. Ví dụ: nếu một giai đoạn có một thành phần và nó không được ưa thích thì`missing = 1`Và`3 * 1 > 1`giữ ngay lập tức, tạo ra sự từ chối ở giai đoạn đầu tiên. Thuật toán xử lý việc này một cách tự nhiên vì bất đẳng thức số nguyên giống nhau được sử dụng bất kể kích thước. 

Khi thành phần còn thiếu bằng đúng 1/3 công đoạn thì công đoạn đó vẫn phải được chấp nhận. Ví dụ: giai đoạn cỡ 3 với 1 thành phần bị thiếu sẽ mang lại`3 * 1 = 3`, không lớn hơn 3. Việc triển khai bảo toàn chính xác điều kiện biên này bằng cách sử dụng bất đẳng thức nghiêm ngặt. 

Nếu không có công đoạn nào chứa thành phần không được ưa thích thì mọi`missing`bằng 0, do đó bất đẳng thức không bao giờ đúng và thuật toán đưa ra kết quả trừ một chính xác sau khi xử lý tất cả các giai đoạn.
