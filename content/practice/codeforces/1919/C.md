---
title: "CF 1919C - Tăng nhóm"
description: "Chúng ta được cấp một mảng và chúng ta phải chia nó thành hai dãy con, gọi chúng là $s$ và $t$, bao gồm mọi phần tử chính xác một lần trong khi vẫn giữ nguyên thứ tự tương đối bên trong mỗi dãy con."
date: "2026-06-08T19:34:27+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1919
codeforces_index: "C"
codeforces_contest_name: "Hello 2024"
rating: 1400
weight: 1919
solve_time_s: 176
verified: true
draft: false
---

[CF 1919C - Tăng nhóm](https://codeforces.com/problemset/problem/1919/C) 

**Đánh giá:** 1400 
**Tags:** cấu trúc dữ liệu, dp, tham lam 
**Thời gian giải:** 2m 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một mảng và chúng ta phải chia nó thành hai dãy con, gọi chúng là$s$Và$t$, bao gồm mọi phần tử chính xác một lần trong khi vẫn giữ nguyên thứ tự tương đối bên trong mỗi chuỗi con. 

Đối với bất kỳ chuỗi nào, hình phạt là số lần chuỗi tăng lên khi di chuyển từ trái sang phải. Vì thế mỗi lần$b_i < b_{i+1}$, chúng tôi trả một đơn vị. 

Mục đích là phân phối các phần tử thành hai chuỗi con sao cho tổng số cặp liền kề tăng dần bên trong cả hai chuỗi được giảm thiểu. 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$tổng các phần tử, do đó, bất kỳ giải pháp nào về cơ bản đều phải tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. DP bậc hai trên tất cả các phần tách hoặc theo dõi tất cả các chuỗi con một cách rõ ràng là không thể. 

Trường hợp cạnh khóa là khi mảng không tăng hoặc không đổi. Trong những trường hợp đó, câu trả lời tối ưu là 0 vì chúng ta có thể đặt mọi thứ vào một dãy con và không bao giờ tạo ra sự gia tăng. Một trường hợp khác là mảng tăng nghiêm ngặt, trong đó bất kỳ chuỗi con nào vẫn sẽ kế thừa cấu trúc tăng dần trừ khi được phân chia cẩn thận, do đó chi phí trở nên không hề nhỏ và buộc phải phân phối tham lam. 

## Phương pháp tiếp cận 

Nếu chúng tôi thử dùng vũ lực, chúng tôi sẽ gán từng phần tử cho một trong hai$s$hoặc$t$, sau đó tính hình phạt. Đó là$2^n$bài tập cho mỗi trường hợp thử nghiệm, điều này ngay lập tức là không thể. 

Một quan điểm có cấu trúc hơn là xử lý các phần tử từ trái sang phải và quyết định nên đặt từng phần tử vào chuỗi nào. Hình phạt chỉ phụ thuộc vào việc phần tử cuối cùng trong chuỗi con có nhỏ hơn phần tử hiện tại hay không, do đó, mỗi chuỗi con hoạt động giống như một chuỗi trong đó các mức tăng là "chuyển tiếp xấu". 

Quan sát quan trọng là mỗi dãy con thực sự là một cấu trúc mà chúng ta muốn tránh tạo ra các cặp liền kề tăng dần. Vì vậy, mỗi dãy con phải hoạt động giống như một dãy “giảm nhất có thể”. 

Điều này dẫn đến ý tưởng tham lam cổ điển: duy trì các giá trị cuối cùng của hai dãy con và luôn gán phần tử hiện tại cho dãy con mà nó gây ra mức tăng hình phạt mới ít nhất. 

Ở mỗi bước, chỉ có hai trạng thái quan trọng, vì vậy bài toán trở thành một tham lam giống DP nhỏ với các chuyển đổi liên tục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n)$|$O(n)$| Quá chậm | 
| Nhiệm vụ tham lam với hai trạng thái |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai biến đại diện cho phần tử cuối cùng của dãy con$s$và tiếp theo$t$. Chúng tôi cũng theo dõi số tiền phạt tích lũy. 

Chúng tôi xử lý các phần tử từ trái sang phải và quyết định vị trí đặt từng giá trị. 

### bước 

1. Khởi tạo cả hai dãy con trống, nghĩa là các phần tử cuối cùng của chúng được đặt thành$+\infty$(vì vậy yếu tố đầu tiên không bao giờ tạo ra hình phạt). 

Điều này đảm bảo nhiệm vụ đầu tiên luôn miễn phí. 
2. Đối với mỗi phần tử$x$, tính chi phí để gắn nó vào$s$và để$t$. 

Chi phí là 1 nếu phần tử cuối cùng của dãy con đó nhỏ hơn hoàn toàn so với$x$, ngược lại là 0. 
3. Chọn dãy con có chi phí nhỏ hơn. Nếu cả hai đều bằng nhau thì chọn tùy ý. 
4. Cập nhật phần tử cuối cùng của dãy con đã chọn thành$x$và thêm chi phí vào câu trả lời. 

### Tại sao nó hoạt động 

Cách duy nhất để tạo ra hình phạt là cục bộ, khi đặt một phần tử sau phần tử nhỏ hơn trước đó trong cùng một chuỗi con. Bất kỳ quyết định nào trong tương lai đều không thay đổi về việc liệu mức tăng này đã xảy ra hay chưa. Điều này làm cho chi phí của mỗi bước trở nên độc lập ngoại trừ các giá trị cuối cùng của hai chuỗi con. Do đó, một lựa chọn tham lam giảm thiểu hình phạt ngay lập tức là tối ưu toàn cầu, vì việc trì hoãn hoặc chuyển sự gia tăng sang tương lai không làm giảm tổng số các vùng lân cận tăng dần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    INF = 10**18

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        last_s = INF
        last_t = INF
        ans = 0

        for x in a:
            cost_s = 1 if last_s < x else 0
            cost_t = 1 if last_t < x else 0

            if cost_s <= cost_t:
                ans += cost_s
                last_s = x
            else:
                ans += cost_t
                last_t = x

        print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện mô phỏng trực tiếp nhiệm vụ tham lam. Chi tiết tinh tế duy nhất là khởi tạo các giá trị cuối cùng thành một số rất lớn để phần tử đầu tiên không bao giờ tạo ra hình phạt. Quy tắc quyết định phải nhất quán: các ràng buộc được phá vỡ một cách an toàn đối với dãy con đầu tiên, nhưng bất kỳ sự phá vỡ ràng buộc nào cũng có tác dụng vì nó không ảnh hưởng đến cấu trúc chi phí trong tương lai ngoài việc hoán đổi các trạng thái đối xứng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Mảng:$[1,2,3,4,5]$| Bước | x | cuối_s | cuối_t | chi phí_s | chi phí_t | đã chọn | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | INF | INF | 0 | 0 | s | 0 | 
| 2 | 2 | 1 | INF | 1 | 0 | t | 0 | 
| 3 | 3 | 1 | 2 | 1 | 1 | s | 1 | 
| 4 | 4 | 3 | 2 | 1 | 1 | s | 2 | 
| 5 | 5 | 4 | 2 | 1 | 1 | s | 3 | 

Điều này cho thấy cách các lần chạy tăng dần được phân chia thành các chuỗi con, buộc tổng cộng phải có chính xác ba lần tăng. 

### Ví dụ 2 

Mảng:$[3,3,3,3]$| Bước | x | cuối_s | cuối_t | chi phí_s | chi phí_t | đã chọn | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 3 | INF | INF | 0 | 0 | s | 0 | 
| 2 | 3 | 3 | INF | 0 | 0 | t | 0 | 
| 3 | 3 | 3 | 3 | 0 | 0 | s | 0 | 
| 4 | 3 | 3 | 3 | 0 | 0 | t | 0 | 

Không có sự gia tăng nào xuất hiện vì không có cặp liền kề tăng nghiêm ngặt nào được hình thành. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi bài kiểm tra | Mỗi phần tử được xử lý một lần với công việc liên tục | 
| Không gian |$O(1)$| Chỉ có hai biến trạng thái được duy trì | 

Tổng độ phức tạp trong tất cả các trường hợp thử nghiệm là tuyến tính ở kích thước đầu vào, phù hợp thoải mái trong các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        last_s = 10**18
        last_t = 10**18
        ans = 0

        for x in a:
            cs = 1 if last_s < x else 0
            ct = 1 if last_t < x else 0
            if cs <= ct:
                ans += cs
                last_s = x
            else:
                ans += ct
                last_t = x

        out.append(str(ans))

    return "\n".join(out)

# provided samples
assert run("""5
5
1 2 3 4 5
8
8 2 3 1 1 7 4 3
5
3 3 3 3 3
1
1
2
2 1
""") == """3
1
0
0
0"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mảng tăng dần | 3 | chia tách buộc phải tăng | 
| mảng giảm dần | 0 | không thể bị phạt | 
| mảng không đổi | 0 | trùng lặp không đóng góp | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi các giá trị lên xuống xen kẽ nhau. Trong những trường hợp như vậy, phép gán tham lam đảm bảo mỗi lần tăng được “hấp thụ” vào nhiều nhất một chuỗi con, ngăn ngừa sự trùng lặp của các hình phạt. Một trường hợp khác là khi có nhiều giá trị bằng nhau xuất hiện, trong đó không có chuỗi con nào phát sinh chi phí, xác nhận rằng sự bằng nhau không ảnh hưởng đến việc hình thành hình phạt.
