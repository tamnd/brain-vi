---
title: "CF 1866A - Đứa Trẻ đầy Tham Vọng"
description: "Chúng ta được cung cấp một danh sách các số nguyên và chúng ta có thể sửa đổi bất kỳ phần tử nào bằng cách tăng hoặc giảm liên tục từng phần tử. Mỗi lần thay đổi đơn vị như vậy sẽ tốn một thao tác."
date: "2026-06-08T23:45:47+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1866
codeforces_index: "A"
codeforces_contest_name: "COMPFEST 15 - Preliminary Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 800
weight: 1866
solve_time_s: 192
verified: true
draft: false
---

[CF 1866A - Đứa trẻ đầy tham vọng](https://codeforces.com/problemset/problem/1866/A) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 3 phút 12s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các số nguyên và chúng ta có thể sửa đổi bất kỳ phần tử nào bằng cách tăng hoặc giảm liên tục từng phần tử. Mỗi lần thay đổi đơn vị như vậy sẽ tốn một thao tác. Mục tiêu không phải là định hình mảng thành một cấu hình cụ thể mà chỉ để đảm bảo rằng tích của tất cả các phần tử sẽ bằng 0. 

Một tích trở thành 0 khi và chỉ khi có ít nhất một phần tử trở thành 0. Điều đó làm giảm toàn bộ nhiệm vụ chỉ còn việc chọn một vị trí duy nhất trong mảng và trả chi phí để chuyển giá trị của nó thành 0, đồng thời bỏ qua tất cả các phần tử khác vì chúng không ảnh hưởng đến sản phẩm khi tồn tại một số 0. 

Chi phí để biến một giá trị thành 0 chỉ đơn giản là giá trị tuyệt đối của nó, vì mỗi bước sẽ di chuyển nó đến gần hơn một. 

Các ràng buộc cho phép tối đa 10^5 phần tử, điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng mô phỏng các hoạt động hoặc khám phá sự kết hợp của các phép biến đổi. Quét tuyến tính tính toán giá trị tuyệt đối tối thiểu là đủ. 

Trường hợp tinh tế chính xuất hiện khi số 0 đã tồn tại. Nếu bất kỳ phần tử nào đã bằng 0 thì câu trả lời là 0 vì sản phẩm đã bằng 0 mà không cần bất kỳ thao tác nào. Một giải pháp bất cẩn bỏ qua trường hợp này và luôn tính giá trị tuyệt đối nhỏ nhất lớn hơn 0 sẽ đánh giá quá cao câu trả lời. 

## Phương pháp tiếp cận 

Giải thích bạo lực sẽ cố gắng chọn từng chỉ số làm mục tiêu trở thành 0 và tính chi phí để làm cho phần tử đó bằng 0, sau đó lấy mức tối thiểu. Điều này vẫn đúng và chạy theo thời gian tuyến tính, vì chi phí của mỗi ứng viên được tính theo thời gian không đổi. Tuy nhiên, ngây thơ hơn nữa, người ta có thể tưởng tượng việc phân phối các thay đổi trên nhiều phần tử, nhưng điều đó là không cần thiết vì việc tạo ra nhiều hơn một phần tử bằng 0 không thể cải thiện chi phí vượt quá phần tử đơn lẻ rẻ nhất. 

Quan sát chính là ràng buộc sản phẩm không yêu cầu cân bằng cấu trúc trên toàn mảng. Một số 0 duy nhất là đủ và việc đưa thêm các số 0 chỉ làm tăng hoặc duy trì chi phí mà không cải thiện được mục tiêu. Điều này thu gọn vấn đề thành việc tìm ra phần tử rẻ nhất để loại bỏ. 

Do đó, vấn đề giảm xuống còn việc tính giá trị tuyệt đối tối thiểu trong mảng, với trường hợp đặc biệt cho số 0 đã tồn tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các chỉ số | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu (giá trị tuyệt đối tối thiểu) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng và theo dõi giá trị tuyệt đối nhỏ nhất được thấy cho đến nay. 

1. Khởi tạo biến trả lời có giá trị rất lớn. Điều này thể hiện chi phí tốt nhất để biến ít nhất một phần tử thành 0. 
2. Lặp qua từng phần tử trong mảng. 
3. Nếu phần tử hiện tại bằng 0, chúng ta có thể đặt ngay câu trả lời thành 0 vì không cần thực hiện thao tác nào nhưng chúng ta vẫn tiếp tục quét để tìm tính đầy đủ. 
4. Mặt khác, hãy tính giá trị tuyệt đối của phần tử, biểu thị số thao tác ±1 cần thiết để biến nó về 0. 
5. Cập nhật câu trả lời với giá trị tối thiểu của giá trị hiện tại và chi phí được tính toán này. 
6. Sau khi xử lý tất cả các phần tử, xuất ra câu trả lời. 

Lý do điều này có hiệu quả là vì mọi giải pháp hợp lệ đều tương ứng với việc chọn một phần tử trở thành 0 và chi phí của lựa chọn đó chính xác là giá trị tuyệt đối của nó. Không có sự tương tác nào tồn tại giữa các phần tử, do đó việc giảm thiểu cục bộ là tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    arr = list(map(int, input().split()))

    ans = float('inf')

    for x in arr:
        if x == 0:
            ans = 0
        else:
            ans = min(ans, abs(x))

    print(ans)

if __name__ == "__main__":
    solve()
```Mã trực tiếp phản ánh lý do. Chúng tôi đọc mảng, theo dõi giá trị tuyệt đối nhỏ nhất và coi số 0 là giải pháp tối ưu trước mắt. Chi tiết triển khai quan trọng là giữ cho quá trình quét đơn giản và tránh các cấu trúc không cần thiết vì chỉ cần một phép tính tối thiểu duy nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
2 -6 5
```Chúng tôi theo dõi giá trị tuyệt đối tối thiểu: 

| Yếu tố | Giá trị | abs(Giá trị) | Câu trả lời hay nhất | 
| --- | --- | --- | --- | 
| 2 | 2 | 2 | 2 | 
| -6 | -6 | 6 | 2 | 
| 5 | 5 | 5 | 2 | 

Câu trả lời cuối cùng là 2. 

Điều này cho thấy việc chọn phần tử 2 là tối ưu vì nó gần bằng 0 nhất. 

### Ví dụ 2 

đầu vào:```
5
0 3 -4 2 10
```| Yếu tố | Giá trị | abs(Giá trị) | Câu trả lời hay nhất | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | 
| 3 | 3 | 3 | 0 | 
| -4 | -4 | 4 | 0 | 
| 2 | 2 | 2 | 0 | 
| 10 | 10 | 10 | 0 | 

Sự hiện diện của số 0 ngay lập tức buộc câu trả lời về số 0. 

Điều này thể hiện trường hợp lợi thế vượt trội trong đó chiến lược tối ưu không yêu cầu thao tác nào cả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý một lần để tính giá trị tuyệt đối của nó | 
| Không gian | O(1) | Chỉ sử dụng một số lượng biến không đổi | 

Quét tuyến tính là đủ cho kích thước đầu vào tối đa, vì các thao tác 10^5 đều nằm trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n = int(sys.stdin.readline())
    arr = list(map(int, sys.stdin.readline().split()))

    ans = float('inf')
    for x in arr:
        if x == 0:
            ans = 0
        else:
            ans = min(ans, abs(x))

    return str(ans)

# provided sample
assert run("""3
2 -6 5
""") == "2"

# all zeros
assert run("""4
0 0 0 0
""") == "0"

# single element positive
assert run("""1
7
""") == "7"

# single element negative
assert run("""1
-9
""") == "9"

# mixed
assert run("""5
3 -1 4 -2 10
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả số không | 0 | trường hợp không ngay lập tức | 
| tích cực duy nhất | 7 | giá trị tuyệt đối cơ sở | 
| âm đơn | 9 | tính đối xứng của chi phí | 
| giá trị hỗn hợp | 1 | lựa chọn tối thiểu đúng | 

## Vỏ cạnh 

Trường hợp cạnh khóa xảy ra khi số 0 đã có sẵn trong mảng. Trong trường hợp đó, tích đã bằng 0 và mọi thao tác tiếp theo đều không cần thiết. Thuật toán xử lý vấn đề này một cách chính xác bằng cách đặt ngay câu trả lời về 0 khi gặp số 0. 

Một trường hợp khác là khi tất cả các giá trị khác xa 0, ví dụ như số dương và số âm lớn. Thuật toán vẫn hoạt động chính xác vì nó chỉ phụ thuộc vào khoảng cách tuyệt đối đến 0 và không tồn tại sự hủy bỏ hoặc tương tác giữa các phần tử. 

Trường hợp tinh tế cuối cùng là khi nhiều phần tử có cùng giá trị tuyệt đối tối thiểu. Thuật toán xử lý vấn đề này một cách tự nhiên vì việc lấy mức tối thiểu không phụ thuộc vào tính duy nhất mà chỉ dựa vào so sánh giá trị.
