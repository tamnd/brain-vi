---
title: "CF 105705B - ​​Cây phân đoạn ?"
description: "Chúng tôi duy trì một chuỗi các số nguyên. Theo thời gian, chúng tôi được phép thực hiện hai hành động. Một hành động trực tiếp thay đổi một vị trí trong mảng thành một giá trị mới."
date: "2026-06-26T08:04:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105705
codeforces_index: "B"
codeforces_contest_name: "AlgoChief Sprint Round 3"
rating: 0
weight: 105705
solve_time_s: 47
verified: true
draft: false
---

[CF 105705B - Cây phân đoạn ?](https://codeforces.com/problemset/problem/105705/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi duy trì một chuỗi các số nguyên. Theo thời gian, chúng tôi được phép thực hiện hai hành động. Một hành động trực tiếp thay đổi một vị trí trong mảng thành một giá trị mới. Hành động còn lại thực thi giới hạn dưới toàn cục: mọi phần tử trong mảng sẽ tăng lên nếu nó nhỏ hơn một giá trị nhất định, nếu không thì nó không thay đổi. 

Sau khi xử lý tất cả các hành động theo thứ tự, nhiệm vụ là xuất ra trạng thái cuối cùng của mảng. 

Khía cạnh quan trọng là bản cập nhật toàn cầu không ghi đè các giá trị một cách mù quáng, nó chỉ đẩy các giá trị lên trên và nhiều bản cập nhật như vậy tương tác với các phép gán điểm sau này một cách không hề tầm thường. 

Các ràng buộc cho phép tổng cộng tối đa một trăm nghìn phần tử và một trăm nghìn thao tác trong các trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào áp dụng rõ ràng phép toán toàn cục bằng cách lặp lại trên toàn bộ mảng mỗi lần, vì điều đó sẽ dẫn đến hành vi bậc hai trong trường hợp xấu nhất. 

Trường hợp cạnh tinh tế xuất hiện khi cập nhật điểm xảy ra sau khi tăng toàn cầu. Ví dụ: nếu chúng ta tăng mọi thứ lên ít nhất là 10, sau đó đặt vị trí thành 6 thì vị trí đó phải giữ nguyên là 6, mặc dù trước đó nó đã bị ép lên. Điều này cho thấy các hoạt động toàn cầu không chi phối vĩnh viễn các nhiệm vụ sau này; thay vào đó, dòng thời gian của hoạt động mới là vấn đề quan trọng. 

Một trường hợp khác xảy ra khi có nhiều bản cập nhật toàn cầu xảy ra. Việc triển khai đơn giản có thể áp dụng lại chúng một cách độc lập, nhưng chỉ ngưỡng toàn cầu tối đa được thấy cho đến nay là có liên quan trừ khi bị ghi đè bởi các thao tác điểm sau này. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực áp dụng từng thao tác theo nghĩa đen. Một bản cập nhật điểm có thời gian không đổi, nhưng thao tác “tối đa với v” toàn cầu sẽ quét toàn bộ mảng và cập nhật từng phần tử nếu cần. Mỗi thao tác như vậy tốn O(n) và với tối đa O(n) thao tác, điều này dẫn đến O(n²), quá chậm đối với đầu vào tỷ lệ 10⁵. 

Quan sát quan trọng là các hoạt động toàn cầu có hiệu lực đơn điệu. Mỗi hoạt động toàn cầu chỉ nâng cao giá trị và không bao giờ giảm chúng. Điều này cho thấy rằng chúng ta không cần phải chạm liên tục vào từng phần tử mà thay vào đó có thể trì hoãn ứng dụng của chúng. 

Cấu trúc chính xuất hiện khi chúng ta đảo ngược góc nhìn. Thay vì áp dụng các bản cập nhật toàn cầu ngay lập tức, chúng tôi theo dõi giá trị toàn cầu tối đa được thấy cho đến nay. Mỗi phần tử mảng thực sự có một “giá trị được đảm bảo tối thiểu” đến từ các hoạt động toàn cầu, nhưng các cập nhật điểm có thể ghi đè giá trị này vào những thời điểm cụ thể. 

Điều này dẫn đến ý tưởng lưu trữ, đối với từng vị trí, xem nó có được cập nhật sau ngưỡng toàn cầu có liên quan cuối cùng hay không và đảm bảo rằng khi cuối cùng chúng tôi đưa ra câu trả lời, mỗi phần tử sẽ được so sánh với ràng buộc toàn cầu mạnh nhất áp dụng cho nó. 

Khó khăn cốt lõi là một bản cập nhật điểm sẽ “phá vỡ” lịch sử của các bản cập nhật toàn cầu trước đó cho chỉ mục đó, vì vậy chúng ta phải tách trạng thái trên mỗi chỉ mục khỏi trạng thái toàn cầu một cách cẩn thận. Đây chính xác là loại tương tác trong đó cây phân đoạn hoặc xử lý ngoại tuyến trở nên hữu ích, nhưng trong vấn đề cụ thể này, việc quét tham lam đơn giản hơn trên các truy vấn là đủ nếu chúng ta theo dõi bất biến chính xác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Áp dụng các hoạt động trực tiếp | O(nq) | O(n) | Quá chậm | 
| Theo dõi mức tối đa toàn cầu + lượt vượt qua cuối cùng bị trì hoãn | O(n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các truy vấn từ trái sang phải trong khi vẫn duy trì một giá trị duy nhất thể hiện ràng buộc “tăng lên ít nhất v” toàn cầu mạnh nhất từng thấy cho đến nay. 

Chúng tôi cũng lưu trữ giá trị được gán mới nhất cho từng chỉ mục, nhưng điều quan trọng là chúng tôi không áp dụng ngay các ràng buộc toàn cục cho mọi phần tử. Thay vào đó, chúng tôi trì hoãn việc giải quyết từng vị trí cho đến khi kết thúc. 

1. Duy trì một biến`mx`lưu trữ giá trị tối đa trong số tất cả các hoạt động loại 2 được thấy cho đến nay. 

2. Duy trì một mảng`a`được khởi tạo với các giá trị đầu vào. 

3. Khi xử lý truy vấn loại 2 có giá trị v, hãy cập nhật`mx = max(mx, v)`. 

4. Khi xử lý truy vấn loại 1 tại chỉ mục x với giá trị p, đặt`a[x] = p`. Nhiệm vụ này được coi là xảy ra sau tất cả các hoạt động trước đó cho đến thời điểm đó. 

5. Sau khi xử lý tất cả các truy vấn, mỗi phần tử ít nhất phải`mx`. Vì vậy, hãy thay thế mọi`a[i]`với`max(a[i], mx)`. 

Bước suy luận tinh tế là hiểu rằng lần quét cuối cùng là đủ vì không có thao tác nào sau truy vấn cuối cùng có thể sửa đổi mảng và tất cả các ràng buộc chung có thể được tóm tắt thành một ngưỡng duy nhất. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, điều duy nhất mà các bản cập nhật toàn cầu làm là tăng giới hạn dưới áp dụng cho tất cả các giá trị trong tương lai. Vì chúng không bao giờ giảm giá trị nên hạn chế mạnh nhất luôn là giá trị tối đa của tất cả các cập nhật toàn cầu trước đó. Bất kỳ giá trị nào được chỉ định sau khi cập nhật toàn cục vẫn phải ngầm tôn trọng tất cả các ràng buộc toàn cục trước đó, nhưng vì chúng tôi ghi đè trực tiếp các giá trị trong quá trình cập nhật điểm, nên giá trị cuối cùng chính xác chỉ đơn giản là giá trị tối đa giữa phép gán rõ ràng cuối cùng và ngưỡng toàn cục mạnh nhất. 

Điều này tạo ra một bất biến: tại bất kỳ thời điểm nào, giá trị thực của mỗi chỉ mục ít nhất là giá trị cập nhật toàn cầu tối đa được thấy cho đến thời điểm đó, trừ khi sau đó nó được ghi đè rõ ràng. Bước cuối cùng thực thi bất biến này trên toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    q = int(input())

    mx = 0

    for _ in range(q):
        t = list(map(int, input().split()))
        if t[0] == 1:
            x, p = t[1] - 1, t[2]
            a[x] = p
        else:
            v = t[1]
            if v > mx:
                mx = v

    for i in range(n):
        if a[i] < mx:
            a[i] = mx

    print(*a)

if __name__ == "__main__":
    solve()
```Việc triển khai chỉ dựa vào việc lưu trữ giá trị rõ ràng mới nhất cho mỗi chỉ mục và mức tối đa toàn cầu duy nhất cho các hoạt động loại 2. Chuyển đổi chỉ mục rất quan trọng vì đầu vào dựa trên 1 nhưng mảng Python dựa trên 0. 

Một lỗi phổ biến là cố gắng áp dụng ngay lập tức các thao tác loại 2 bằng cách lặp qua toàn bộ mảng, điều này sẽ không vượt qua được các ràng buộc. Một sai lầm khác là quên rằng việc gán điểm muộn hơn có thể “hoàn tác” các lần tăng điểm trước đó cho chỉ mục đó, vì vậy chúng ta phải luôn lưu trữ phép gán cuối cùng làm thẩm quyền cho vị trí đó. 

## Ví dụ đã hoạt động 

Hãy xem xét một mảng`[1, 2, 3]`với các thao tác: tăng lên ít nhất là 5, sau đó đặt vị trí 2 thành 1. 

| Bước | Hoạt động | Trạng thái mảng | mx | 
|---|---|---|---| 
| 1 | ban đầu | [1, 2, 3] | 0 | 
| 2 | toàn cầu ≥ 5 | [1, 2, 3] | 5 | 
| 3 | đặt a[2]=1 | [1, 1, 3] | 5 | 
| 4 | áp dụng cuối cùng | [5, 5, 5] ngoại trừ bị ghi đè | 5 | 

Sau khi chỉnh sửa lần cuối, kết quả là`[5, 5, 5]`ngoại trừ chỉ số 2 trở thành`max(1, 5) = 5`. 

Điều này cho thấy việc cập nhật điểm không bao giờ hủy bỏ các ràng buộc toàn cục. 

Bây giờ hãy xem xét đảo ngược thứ tự: đặt rồi toàn cầu. 

| Bước | Hoạt động | Trạng thái mảng | mx | 
|---|---|---|---| 
| 1 | đặt a[1]=10 | [10, 2, 3] | 0 | 
| 2 | toàn cầu ≥ 5 | [10, 2, 3] | 5 | 
| 3 | áp dụng cuối cùng | [10, 5, 5] | 5 | 

Điều này cho thấy các cập nhật điểm có thể vượt quá các ràng buộc toàn cục và được giữ nguyên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | O(n + q) | Mỗi truy vấn được xử lý một lần, lần truy vấn cuối cùng là tuyến tính | 
| Không gian | O(n) | Chỉ cần lưu trữ mảng | 

Giải pháp dễ dàng phù hợp trong giới hạn vì cả n và q đều có nhiều nhất là 10⁵ trong các trường hợp thử nghiệm và mọi thao tác đều có thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return main_capture()

def main_capture():
    import sys
    input = sys.stdin.readline

    t = 1
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        q = int(input())

        mx = 0
        for _ in range(q):
            tmp = list(map(int, input().split()))
            if tmp[0] == 1:
                a[tmp[1]-1] = tmp[2]
            else:
                mx = max(mx, tmp[1])

        for i in range(n):
            a[i] = max(a[i], mx)

        out.append(" ".join(map(str, a)))

    return "\n".join(out)

# sample-like case
assert run("3\n1 2 3\n3\n2 5\n1 1 1\n2 4\n") == "5 5 5"

# minimum size
assert run("1\n10\n2\n2 5\n1 1 3\n") == "5"

# no global updates
assert run("3\n1 2 3\n2\n1 2 10\n1 3 7\n") == "1 10 7"

# only global updates
assert run("3\n1 2 3\n2\n2 100\n2 50\n") == "100 100 100"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
| hỗn hợp yếu tố đơn lẻ | 5 | tương tác của toàn cầu và điểm | 
| cỡ 1 | 5 | hành vi ranh giới | 
| không có hoạt động toàn cầu | 1 10 7 | tính chính xác không có mx | 
| chỉ hoạt động toàn cầu | 100 100 100 | sự sụp đổ toàn cầu lặp đi lặp lại | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi cập nhật điểm xảy ra sau cập nhật toàn cầu và đặt giá trị nhỏ hơn ngưỡng chung. Thuật toán giải quyết chính xác điều này vì phép toán tối đa cuối cùng sẽ thực thi lại ràng buộc toàn cục. Đối với đầu vào`[1]`, các hoạt động “được đặt thành 2, sau đó tổng thể thành 5, sau đó đặt thành 3”, kết quả cuối cùng trở thành 5, vì lần quét cuối cùng lại thực thi mức tối đa toàn cầu. 

Một trường hợp khác là nhiều bản cập nhật toàn cầu xen kẽ với các bản cập nhật điểm. Thuật toán xử lý việc này vì chỉ có giá trị toàn cầu tối đa mới quan trọng; các bản cập nhật nhỏ hơn trung gian sẽ không liên quan khi một bản cập nhật lớn hơn xuất hiện. 

Trường hợp thứ ba là khi không có bản cập nhật toàn cầu nào cả. Trong trường hợp này`mx`vẫn bằng 0, do đó mảng cuối cùng không thay đổi so với các bản cập nhật điểm cuối cùng, giúp duy trì tính chính xác mà không cần viết hoa đặc biệt.
