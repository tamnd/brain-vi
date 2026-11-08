---
title: "CF 1426D - Phân đoạn khác 0"
description: "Chúng ta được cấp một chuỗi các số nguyên khác 0 và được phép chèn các số nguyên tùy ý vào bất kỳ vị trí nào giữa các phần tử liền kề. Mục tiêu là sửa đổi trình tự sao cho không có phân đoạn liền kề nào của mảng cuối cùng có tổng chính xác bằng 0, đồng thời sử dụng càng ít phần tử được chèn càng tốt."
date: "2026-06-11T05:46:24+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "data-structures", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1426
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 674 (Div. 3)"
rating: 1500
weight: 1426
solve_time_s: 71
verified: true
draft: false
---

[CF 1426D - Phân đoạn khác 0](https://codeforces.com/problemset/problem/1426/D) 

**Đánh giá:** 1500 
**Tags:** thuật toán xây dựng, cấu trúc dữ liệu, tham lam, sắp xếp 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi các số nguyên khác 0 và được phép chèn các số nguyên tùy ý vào bất kỳ vị trí nào giữa các phần tử liền kề. Mục tiêu là sửa đổi trình tự sao cho không có phân đoạn liền kề nào của mảng cuối cùng có tổng chính xác bằng 0, đồng thời sử dụng càng ít phần tử được chèn càng tốt. 

Tổng của mảng con bằng 0 là dễ hiểu nhất thông qua tổng tiền tố. Nếu hai vị trí trong mảng có tổng tiền tố giống nhau thì tổng các phần tử giữa chúng bằng 0. Vì vậy, vấn đề tương đương với việc ngăn chặn mọi giá trị tổng tiền tố lặp lại khi chúng ta quét từ trái sang phải, sau khi chúng ta được phép “phá vỡ” mảng bằng cách chèn các phần tử mới. 

Ràng buộc$n \le 2 \cdot 10^5$ngụ ý rằng chúng ta cần một giải pháp tuyến tính hoặc gần tuyến tính. Bất kỳ cách tiếp cận nào kiểm tra rõ ràng tất cả các mảng con hoặc tính toán lại tổng tiền tố trên các mảng đã sửa đổi sẽ quá chậm vì việc chèn các phần tử sẽ thay đổi cấu trúc một cách linh hoạt và có thể dẫn đến sự lan truyền bậc hai. 

Một trường hợp cạnh tinh tế phát sinh khi nhiều phân đoạn rời rạc tạo ra các khoảng có tổng bằng 0 một cách độc lập. Ví dụ: trong một mảng như$[1, -1, 2, -2]$, cả hai$[1, -1]$Và$[2, -2]$hình thành các phân đoạn có tổng bằng 0. Một kẻ tham lam ngây thơ chỉ sửa lỗi xảy ra lần đầu tiên có thể bỏ lỡ các xung đột sau này trừ khi nó theo dõi hành vi tiền tố chung một cách chính xác. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô phỏng tất cả các cách có thể để chèn các phần tử và sau đó kiểm tra xem mảng kết quả có bất kỳ phân đoạn có tổng bằng 0 nào không. Ngay cả việc hạn chế “chèn hoặc không chèn giữa mỗi cặp” vẫn dẫn đến khả năng theo cấp số nhân. Ngay cả khi chúng tôi sửa một mảng ứng cử viên, việc xác minh nó yêu cầu tổng tiền tố và tập hợp băm tuyến tính. Nhưng việc khám phá tất cả các cấu hình chèn là không thể vì số lượng cấu hình tăng theo cấp số nhân với$n$. 

Quan sát quan trọng là các mảng con có tổng bằng 0 hoàn toàn được xác định bằng cách lặp lại tổng tiền tố. Trong khi quét mảng, chúng tôi duy trì tổng tiền tố hiện tại. Nếu chúng ta đạt đến tổng tiền tố mà chúng ta đã thấy, điều đó có nghĩa là tồn tại một mảng con có tổng bằng 0. Để ngăn chặn điều này, chúng ta phải đảm bảo rằng tổng tiền tố trở thành “duy nhất” sau khi lặp lại. 

Việc chèn một phần tử sẽ đặt lại tính liên tục của tổng tiền tố một cách hiệu quả. Về mặt khái niệm, mỗi lần chèn sẽ bắt đầu một “phân đoạn” mới có lịch sử tổng tiền tố là độc lập. Khi chúng tôi phân chia tại một thời điểm nào đó, tổng tiền tố trước và sau không còn được so sánh nữa. 

Do đó, vấn đề giảm xuống còn việc duy trì một tập hợp các tổng tiền tố cho phân đoạn hiện tại. Khi duyệt mảng, bất cứ khi nào tổng tiền tố lặp lại trong phân đoạn hiện tại, chúng ta phải cắt phân đoạn đó trước khi tiếp tục, tương ứng với việc chèn một phần tử. Sau khi cắt, chúng tôi khởi động lại các tổng tiền tố theo dõi từ 0. 

Sự phân đoạn tham lam này có tác dụng vì việc trì hoãn việc cắt chỉ làm tăng nguy cơ va chạm tiếp theo mà không làm giảm các va chạm trong tương lai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (thử tất cả các phần chèn thêm) | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu (đặt lại tiền tố tham lam) | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng từ trái sang phải trong khi duy trì tổng tiền tố đang chạy và tập hợp tổng tiền tố được thấy trong phân đoạn hiện tại. 

1. Bắt đầu với tổng tiền tố bằng 0 và một tập hợp trống chỉ chứa 0. Điều này thể hiện tiền tố trống trước khi bất kỳ phần tử nào được xử lý. 
2. Lặp lại từng phần tử của mảng, thêm nó vào tổng tiền tố hiện tại. 
3. Nếu tổng tiền tố được cập nhật chưa được nhìn thấy trong phân đoạn hiện tại, hãy chèn nó vào tập hợp và tiếp tục. 
4. Nếu tổng tiền tố đã được nhìn thấy, điều này có nghĩa là tồn tại một mảng con trong phân đoạn hiện tại có tổng bằng 0. Để loại bỏ điều này, về mặt khái niệm, chúng tôi sẽ chèn một số trước vị trí hiện tại để đặt lại phân đoạn. 
5. Tăng câu trả lời lên một, xóa tập hợp và khởi động lại phân đoạn với tổng tiền tố chỉ được đặt lại về giá trị hiện tại, chèn nó vào tập hợp mới. 
6. Tiếp tục cho đến hết mảng. 

### Tại sao nó hoạt động 

Điều bất biến là trong mỗi phân đoạn hoạt động, tất cả các tổng tiền tố đều khác biệt. Bất kỳ sự lặp lại nào bên trong một phân đoạn đều tương ứng chính xác với phân mảng có tổng bằng 0 hoàn toàn chứa trong phân đoạn đó. Vì chúng tôi không thể sửa các giá trị trong quá khứ nên cách duy nhất để loại bỏ xung đột đó là cắt phân đoạn. Mỗi lần cắt sẽ loại bỏ tất cả lịch sử tiền tố trước đó, vì vậy chúng tôi không bao giờ chuyển tiếp tiền tố xung đột được đặt trước. Việc cắt tham lam ở lần lặp lại đầu tiên đảm bảo chúng tôi không bao giờ tạo nhiều phân đoạn hơn mức cần thiết, vì bất kỳ lần cắt nào sau đó vẫn cần phải tách cặp tiền tố xung đột giống nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    seen = set([0])
    prefix = 0
    cuts = 0

    for x in a:
        prefix += x
        if prefix in seen:
            cuts += 1
            seen = set([0])
            prefix = x
            seen.add(prefix)
        else:
            seen.add(prefix)

    print(cuts)

if __name__ == "__main__":
    solve()
```Giải pháp duy trì tổng tiền tố đang chạy và tập hợp băm các tổng tiền tố được thấy trong phân đoạn hiện tại. Bước đặt lại là thao tác chính: khi xảy ra sự lặp lại, chúng tôi tăng câu trả lời và bắt đầu theo dõi lại từ phần tử hiện tại làm phần tử đầu tiên của phân đoạn mới. Điều này phản ánh ý tưởng rằng việc chèn một số sẽ phá vỡ tính liên tục của tiền tố một cách hiệu quả. 

Một điểm tinh tế là sau khi đặt lại, chúng tôi coi phần tử hiện tại là tiền tố đầu tiên trong phân đoạn mới, do đó tiền tố không được đặt lại về 0 mà được khởi tạo lại một cách chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu:```
4
1 -5 3 2
```Chúng tôi theo dõi tổng tiền tố và tập hợp: 

| Bước | Yếu tố | Tiền tố | Đã Xem Bộ | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | {0, 1} | tiếp tục | 
| 2 | -5 | -4 | {0, 1, -4} | tiếp tục | 
| 3 | 3 | -1 | {0, 1, -4, -1} | tiếp tục | 
| 4 | 2 | 1 | sự lặp lại | cắt = 1, đặt lại | 

Ở bước 4, tiền tố tổng 1 lặp lại, biểu thị phân đoạn có tổng bằng 0 tồn tại trong phân đoạn hiện tại. Chúng tôi chèn một phần tử và khởi động lại. 

Sau khi thiết lập lại: 

| Bước | Yếu tố | Tiền tố | Đã Xem Bộ | Hành động | 
| --- | --- | --- | --- | --- | 
| 4 (khởi động lại) | 2 | 2 | {0, 2} | tiếp tục | 

Câu trả lời cuối cùng là 1. 

Dấu vết này cho thấy sự lặp lại tiền tố trực tiếp kích hoạt việc phân chia phân đoạn như thế nào. 

Bây giờ hãy xem xét:```
5
3 -1 -2 4 -4
```| Bước | Yếu tố | Tiền tố | Đã Xem Bộ | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 3 | {0, 3} | tiếp tục | 
| 2 | -1 | 2 | {0, 3, 2} | tiếp tục | 
| 3 | -2 | 0 | lặp lại (0) | cắt = 1 | 
| 3 khởi động lại | -2 | -2 | {0, -2} | tiếp tục | 
| 4 | 4 | 2 | {0, -2, 2} | tiếp tục | 
| 5 | -4 | -2 | sự lặp lại | cắt = 2 | 

Chúng tôi cần hai lần cắt để ngăn tổng tiền tố lặp lại. 

Điều này chứng tỏ rằng nhiều vùng có tổng bằng 0 độc lập yêu cầu ngắt phân đoạn riêng biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý một lần và các thao tác thiết lập là trung bình O(1) | 
| Không gian | O(n) | Bộ tiền tố lưu trữ tối đa một mục nhập cho mỗi phần tử trong một phân đoạn | 

Giải pháp là tuyến tính và phù hợp thoải mái trong các ràng buộc lên đến$2 \cdot 10^5$, cả về thời gian chạy và mức sử dụng bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return str(solve_output(inp))

# re-implementation wrapper for testing
def solve_output(inp: str) -> str:
    data = inp.strip().split()
    n = int(data[0])
    a = list(map(int, data[1:]))

    seen = set([0])
    prefix = 0
    cuts = 0

    for x in a:
        prefix += x
        if prefix in seen:
            cuts += 1
            seen = set([0])
            prefix = x
            seen.add(prefix)
        else:
            seen.add(prefix)

    return str(cuts)

# provided sample
assert solve_output("4\n1 -5 3 2\n") == "1"

# minimum size
assert solve_output("2\n1 -1\n") == "1"

# already safe
assert solve_output("3\n1 2 3\n") == "0"

# alternating cancellations
assert solve_output("4\n1 -1 2 -2\n") == "2"

# large no conflict
assert solve_output("5\n1 1 1 1 1\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2, 1 -1 | 1 | phát hiện tổng bằng 0 ngay lập tức | 
| 1 2 3 | 0 | trường hợp không lặp lại | 
| 1 -1 2 -2 | 2 | nhiều phân khúc độc lập | 
| 1 1 1 1 1 | 0 | tăng trưởng tiền tố đơn điệu | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi phân đoạn có tổng bằng 0 kết thúc chính xác tại ranh giới của phân đoạn trước đó. Ví dụ, trong`[1, -1, 1, -1]`, tổng tiền tố lặp lại nhiều lần và mỗi lần lặp lại buộc phải đặt lại. 

Từng bước sau khi xử lý`[1, -1]`, tiền tố trở về 0, kích hoạt cắt. Nếu không đặt lại chính xác, việc triển khai đơn giản sẽ hợp nhất không chính xác lịch sử tiền tố trên các phân đoạn và đánh giá thấp các phần chèn cần thiết. 

Một trường hợp đặc biệt khác là sự lặp lại ngay lập tức sau khi đặt lại, chẳng hạn như`[1, -1, 1]`. Sau khi xử lý`[1, -1]`, chúng tôi thiết lập lại. Khi xử lý cuối cùng`1`, tiền tố trở thành 1, tiền tố này đã hợp lệ trong phân đoạn mới, do đó không cần cắt thêm. Điều này xác nhận rằng việc đặt lại cách ly hoàn toàn lịch sử tiền tố và ngăn chặn nhiễu giữa các phân đoạn.
