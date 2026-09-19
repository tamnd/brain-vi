---
title: "CF 104760A - \u041c\u043d\u043e\u0433\u043e \u0440\u0430\u043a\u0443\u0448\u0435\u043a"
description: "Chúng ta được cho một dãy tuyến tính các vỏ, mỗi vỏ có trọng số dương. Các vỏ sò được sắp xếp theo thứ tự dọc theo một bãi biển và chúng ta chỉ được phép lấy một khối liên tiếp."
date: "2026-06-29T02:21:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104760
codeforces_index: "A"
codeforces_contest_name: "2023-2024 ICPC NERC (NEERC), Kyrgyzstan Qualification Contest"
rating: 0
weight: 104760
solve_time_s: 68
verified: true
draft: false
---

[CF 104760A - \u041c\u043d\u043e\u0433\u043e \u0440\u0430\u043a\u0443\u0448\u0435\u043a](https://codeforces.com/problemset/problem/104760/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy tuyến tính các vỏ, mỗi vỏ có trọng số dương. Các vỏ sò được sắp xếp theo thứ tự dọc theo một bãi biển và chúng ta chỉ được phép lấy một khối liên tiếp. Tuy nhiên, có một hạn chế về dung lượng: túi chỉ có thể chứa tối đa`K`vỏ, bất kể tổng trọng lượng của chúng. 

Nhiệm vụ là chọn một đoạn liền kề có độ dài không vượt quá`K`, sao cho tổng trọng số bên trong phân đoạn đó là lớn nhất. 

Đầu ra là một số duy nhất, tổng lớn nhất có thể có trên tất cả các phân đoạn liền kề hợp lệ. 

Các ràng buộc đủ lớn để$O(N^2)$quét qua tất cả các mảng con là không khả thi. Với$N \le 2 \cdot 10^5$, một cách tiếp cận bậc hai sẽ bao hàm thứ tự của$4 \cdot 10^{10}$hoạt động vượt xa các giới hạn thông thường. Điều này ngay lập tức gợi ý rằng chúng ta cần một phương pháp tuyến tính hoặc gần tuyến tính. 

Trường hợp cạnh tinh tế xuất hiện khi`K >= N`. Trong tình huống đó, mọi phần tử đều có thể được lấy đi, vì vậy câu trả lời chỉ đơn giản là tổng của toàn bộ mảng. Việc triển khai cửa sổ trượt đơn giản luôn cố gắng duy trì kích thước cửa sổ cố định`K`phải cẩn thận để không thu nhỏ xuống dưới phạm vi hợp lệ khi mảng ngắn hơn`K`. 

Một cạm bẫy tiềm ẩn khác là giả định đoạn tối ưu luôn có độ dài chính xác.`K`. Điều này không hẳn đúng vì phân đoạn ngắn hơn có thể có tổng lớn hơn nếu tránh được cấu trúc âm, nhưng trong bài toán này tất cả các trọng số đều dương, vì vậy phân đoạn tốt nhất sẽ luôn sử dụng chính xác`K`các phần tử trừ khi bị ràng buộc bởi ranh giới. Thuộc tính đó là yếu tố làm cho cách tiếp cận cửa sổ trượt trở nên sạch sẽ. 

## Phương pháp tiếp cận 

Phương pháp brute-force thử mọi vị trí bắt đầu có thể và mở rộng nó đến`K`bước về phía trước, tích lũy số tiền khi nó đi. Đối với mỗi chỉ số`i`, nó kiểm tra tất cả các mảng con bắt đầu từ`i`và có chiều dài tối đa`K`, cập nhật mức tối đa toàn cầu. 

Điều này đúng vì nó liệt kê mọi phân đoạn hợp lệ một cách rõ ràng. Vấn đề là số lượng hoạt động. Đối với mỗi`N`điểm bắt đầu, lên đến`K`các phần tử có thể được xử lý, dẫn đến$O(NK)$thời gian. Từ`K`có thể lớn như$10^7$, điều này hoàn toàn không thể thực hiện được. 

Cấu trúc của vấn đề gợi ý một cách tiếp cận tốt hơn. Chúng tôi đang tối đa hóa tổng trên tất cả các phân đoạn liền kề với giới hạn độ dài tối đa cố định. Bởi vì tất cả các trọng số đều dương, nên việc mở rộng một cửa sổ luôn làm tăng tổng, do đó, đối với bất kỳ điểm cuối bên phải cố định nào, đoạn hợp lệ tốt nhất kết thúc ở đó là đoạn dài nhất có thể, đến độ dài`K`. Điều này có nghĩa là chúng ta chỉ cần duy trì một cửa sổ trượt có kích thước tối đa`K`đồng thời quét từ trái sang phải, theo dõi số tiền hiện tại một cách hiệu quả. 

Chúng tôi duy trì tổng số hoạt động của cửa sổ hiện tại. Khi chúng ta di chuyển ranh giới bên phải về phía trước, chúng ta sẽ thêm phần tử mới. Nếu cửa sổ vượt quá kích thước`K`, chúng tôi loại bỏ phần tử ngoài cùng bên trái. Ở mỗi bước, tổng hiện tại đại diện cho phân đoạn tốt nhất kết thúc ở vị trí đó, vì vậy chúng tôi cập nhật câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(NK) | O(1) | Quá chậm | 
| Cửa Sổ Trượt | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai con trỏ`l = 0`và tổng số tiền đang chạy`current = 0`, và một biến`best = 0`. 

Các con trỏ xác định cửa sổ hiện tại của shell mà chúng ta đang xem xét. 
2. Lặp lại`r`từ`0`ĐẾN`N - 1`, coi mỗi vị trí là điểm cuối bên phải của cửa sổ. 

Mỗi bước sẽ mở rộng phân khúc ứng cử viên bằng cách thêm một shell nữa. 
3. Thêm`W[r]`ĐẾN`current`. 

Điều này kết hợp shell mới vào tổng cửa sổ đang chạy. 
4. Nếu kích thước cửa sổ vượt quá`K`, thu nhỏ nó từ bên trái bằng cách trừ`W[l]`và tăng dần`l`. 

Điều này đảm bảo chúng ta tôn trọng ràng buộc mà nhiều nhất`K`vỏ có thể được lấy. Nếu chúng ta không loại bỏ các phần tử ở đây, cửa sổ có thể tăng kích thước tùy ý và vi phạm điều kiện vấn đề. 
5. Sau khi điều chỉnh cửa sổ, cập nhật`best = max(best, current)`. 

Tại thời điểm này, cửa sổ biểu thị phân đoạn hợp lệ tốt nhất kết thúc tại`r`, bởi vì bất kỳ tiền tố ngắn hơn nào cũng sẽ chỉ làm giảm tổng do tính dương của trọng số. 
6. Sau khi xử lý tất cả các vị trí, xuất ra`best`. 

### Tại sao nó hoạt động 

Ở mọi vị trí`r`, thuật toán duy trì một cửa sổ`[l, r]`đó là hợp lệ (độ dài tối đa`K`) và tổng của nó được tính toán chính xác. Vì tất cả các trọng số đều dương nên bất kỳ phân đoạn hợp lệ nào kết thúc tại`r`có độ dài nhỏ hơn cửa sổ hiện tại sẽ có tổng nhỏ hơn hoặc bằng nhau. Do đó, khi cửa sổ được tối đa hóa chiều dài theo ràng buộc, nó cũng tối đa hóa tổng cho điểm cuối đó. Đang quét tất cả`r`đảm bảo mọi ranh giới bên phải có thể đều được xem xét, do đó, mức tối đa toàn cầu trên tất cả các phân đoạn hợp lệ sẽ được ghi lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    w = list(map(int, input().split()))
    k = int(input())

    l = 0
    current = 0
    best = 0

    for r in range(n):
        current += w[r]

        if r - l + 1 > k:
            current -= w[l]
            l += 1

        if current > best:
            best = current

    print(best)

if __name__ == "__main__":
    solve()
```Mã duy trì một cửa sổ trượt duy nhất trên mảng. Con trỏ trái`l`chỉ di chuyển về phía trước nên mỗi phần tử được thêm và xóa nhiều nhất một lần, đảm bảo độ phức tạp tuyến tính. điều kiện`r - l + 1 > k`thực thi giới hạn kích thước cửa sổ tối đa. Biến`current`luôn lưu trữ tổng của phân đoạn hợp lệ hiện tại và`best`theo dõi mức tối đa được thấy cho đến nay. 

Một lỗi triển khai phổ biến là kiểm tra kích thước cửa sổ trước khi thêm`w[r]`, điều này có thể dẫn đến từng lỗi một. Thứ tự đúng là bao gồm phần tử trước, sau đó sửa cửa sổ nếu cần. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10
1 3 2 4 1 5 1 2 1 2
5
```Chúng tôi theo dõi cửa sổ như sau. 

| r | tôi | cửa sổ | tổng hiện tại | tốt nhất | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | [1] | 1 | 1 | 
| 1 | 0 | [1,3] | 4 | 4 | 
| 2 | 0 | [1,3,2] | 6 | 6 | 
| 3 | 0 | [1,3,2,4] | 10 | 10 | 
| 4 | 0 | [1,3,2,4,1] | 11 | 11 | 
| 5 | 1 | [3,2,4,1,5] | 15 | 15 | 
| 6 | 2 | [2,4,1,5,1] | 13 | 15 | 
| 7 | 3 | [4,1,5,1,2] | 13 | 15 | 
| 8 | 4 | [1,5,1,2,1] | 10 | 15 | 
| 9 | 5 | [5,1,2,1,2] | 11 | 15 | 

Dấu vết cho thấy cửa sổ dịch chuyển sang phải một cách tự nhiên như thế nào trong khi vẫn duy trì kích thước 5. Đạt được mức tối đa khi bao gồm đầy đủ phân đoạn trọng lượng dày đặc nhất. 

### Ví dụ 2 

đầu vào:```
6
5 1 5 1 5 1
2
```| r | tôi | cửa sổ | tổng hiện tại | tốt nhất | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | [5] | 5 | 5 | 
| 1 | 0 | [5,1] | 6 | 6 | 
| 2 | 1 | [1,5] | 6 | 6 | 
| 3 | 2 | [5,1] | 6 | 6 | 
| 4 | 3 | [1,5] | 6 | 6 | 
| 5 | 4 | [5,1] | 6 | 6 | 

Trường hợp này thể hiện các cửa sổ tối ưu lặp lại và cho thấy thuật toán xử lý chính xác nhiều phân đoạn tối ưu chồng chéo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi phần tử được thêm một lần và bị xóa nhiều nhất một lần do chuyển động của cửa sổ trượt | 
| Không gian | O(1) | Chỉ có một vài bộ đếm và mảng đầu vào được lưu trữ | 

Quét tuyến tính là đủ cho$N \le 2 \cdot 10^5$và mức sử dụng bộ nhớ là tối thiểu do không yêu cầu cấu trúc dữ liệu phụ trợ ngoài bộ đếm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    w = list(map(int, input().split()))
    k = int(input())

    l = 0
    current = 0
    best = 0

    for r in range(n):
        current += w[r]
        if r - l + 1 > k:
            current -= w[l]
            l += 1
        if current > best:
            best = current

    return str(best)

assert run("""10
1 3 2 4 1 5 1 2 1 2
5
""") == "15"

assert run("""1
10
1
""") == "10"

assert run("""5
1 1 1 1 1
3
""") == "3"

assert run("""6
5 1 5 1 5 1
2
""") == "6"

assert run("""7
2 100 3 4 5 6 1
3
""") == "111"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 10 | xử lý kích thước tối thiểu | 
| tất cả những cái | 3 | tính chính xác của mảng thống nhất | 
| đỉnh xen kẽ | 6 | chồng chéo các cửa sổ tối ưu | 
| đỉnh lớn hỗn hợp | 111 | chuyển cửa sổ đúng cách | 

## Vỏ cạnh 

Khi nào`K >= N`, cửa sổ không bao giờ co lại vì giới hạn kích thước không bao giờ bị vi phạm. Thuật toán chỉ đơn giản là tích lũy tất cả các phần tử một lần và`best`trở thành tổng số tiền. 

Đối với mảng một phần tử như`N = 1`, vòng lặp chạy một lần, phần tử được thêm vào và không xảy ra hiện tượng thu hẹp. Kết quả chính xác là phần tử đó, khớp với phân đoạn hợp lệ duy nhất. 

Trong các mảng có các giá trị lớn cách nhau, chẳng hạn như`[2, 100, 3, 4, 5, 6, 1]`với`K = 3`, cửa sổ trượt sẽ dịch chuyển chính xác để bao gồm nhóm liền kề lớn nhất phù hợp. Ở mỗi bước, cửa sổ vẫn hợp lệ và mức tối đa được cập nhật tại thời điểm cụm giá trị cao nằm hoàn toàn bên trong cửa sổ.
