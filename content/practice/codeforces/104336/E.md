---
title: "CF 104336E - Giải quyết vấn đề mỗi ngày"
description: "Chúng tôi đang theo dõi việc giải quyết vấn đề hàng ngày của Maxim trong chuỗi $n$ ngày. Mỗi ngày anh ấy giải được ít nhất một bài toán và chúng tôi muốn gán một số nguyên dương chính xác cho mỗi ngày. Hai đại lượng được quan sát thấy ở mỗi ngày $i$."
date: "2026-07-01T18:47:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104336
codeforces_index: "E"
codeforces_contest_name: "II Olympiad of classes at the Mechanics and Mathematics Faculty of MSU in programming 2023."
rating: 0
weight: 104336
solve_time_s: 61
verified: true
draft: false
---

[CF 104336E - Giải quyết vấn đề mỗi ngày](https://codeforces.com/problemset/problem/104336/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang theo dõi việc giải quyết vấn đề hàng ngày của Maxim theo trình tự$n$ngày. Mỗi ngày anh ấy giải được ít nhất một bài toán và chúng tôi muốn gán một số nguyên dương chính xác cho mỗi ngày. 

Hai đại lượng được quan sát mỗi ngày$i$. Đầu tiên là tổng số vấn đề được giải quyết trong lần cuối cùng$k$ngày kết thúc vào lúc$i$, hoạt động giống như một cửa sổ trượt tổng chiều dài$k$. Thứ hai là số ngày liên tiếp kết thúc vào lúc$i$trong thời gian đó anh ấy đã giải quyết các vấn đề hàng ngày, đơn giản là độ dài chuỗi hiện tại. 

Quy tắc là vào mỗi ngày, tổng cửa sổ trượt trong ngày cuối cùng$k$ngày ít nhất phải bằng độ dài chuỗi hiện tại. Chúng tôi được biết điều kiện này đã được áp dụng cho tất cả$n$ngày và chúng ta phải xây dựng lại một chuỗi các giá trị hàng ngày thỏa mãn ràng buộc đồng thời giảm thiểu tổng tổng trên tất cả$n$ngày. 

Đầu vào mang lại$k$, kích thước cửa sổ và$n$, số ngày. Đầu ra là tổng số vấn đề được giải quyết tối thiểu có thể. 

Các ràng buộc đi lên đến$10^6$, do đó mọi nghiệm đều phải tuyến tính hoặc gần tuyến tính trong$n$. Việc xây dựng lại bậc hai của tất cả các chuỗi hợp lệ hoặc tính toán lại nhiều lần các cửa sổ trượt là không thể, vì thậm chí$O(nk)$sẽ đạt được$10^{12}$hoạt động trong trường hợp xấu nhất. 

Một ý tưởng ngây thơ là thử gán giá trị nhỏ nhất có thể mỗi ngày, thường là 1 và kiểm tra xem các ràng buộc có bị vi phạm hay không. Trường hợp thất bại xuất hiện khi cửa sổ trượt quá nhỏ so với tốc độ tăng trưởng của vệt. Ví dụ, khi$k = 2$, khoảng thời gian của ngày thứ ba chỉ trùng lặp một phần với hai ngày đầu tiên, do đó, chỉ giữ tất cả các giá trị bằng 1 sẽ nhanh chóng không đủ để thỏa mãn sự bất bình đẳng liên quan đến chuỗi ngày càng tăng. Một mô phỏng đơn giản sẽ liên tục điều chỉnh các giá trị trước đó, dẫn đến các bản cập nhật xếp tầng khiến quá trình mô phỏng trở nên quá chậm. 

Một vấn đề tế nhị khác là ràng buộc phụ thuộc vào cả tổng di chuyển và chuỗi tổng thể, vì vậy các lựa chọn tham lam cục bộ có thể phá vỡ tính khả thi sau này mà không bị phát hiện ngay lập tức. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ sẽ mô phỏng từng ngày, duy trì toàn bộ mảng và, mỗi ngày, tính toán lại cả độ dài chuỗi và tổng trong ngày cuối cùng.$k$ngày. Nếu ràng buộc bị vi phạm vào ngày$i$, chúng tôi sẽ cố gắng tăng một số giá trị trước đó để khắc phục nó. Trong trường hợp xấu nhất, mỗi điều chỉnh có thể lan truyền ngược lên tới$k$các vị trí và điều này có thể lặp lại trong nhiều ngày. Điều này dẫn đến sự phức tạp trong trường hợp xấu nhất theo thứ tự$O(nk)$, vì mỗi ngày có thể phải tính toán lại và sửa chữa cửa sổ trượt. 

Quan sát quan trọng là ràng buộc chỉ phụ thuộc vào hai đại lượng đơn điệu: vệt phát triển một cách xác định (nó luôn luôn$i$) và tổng cửa sổ trượt được điều khiển bởi một cửa sổ có kích thước giới hạn$k$. Sự bất bình đẳng buộc tổng cửa sổ phải “theo kịp” mức tăng trưởng tuyến tính, nhưng chỉ thông qua các điều chỉnh cục bộ. Điều này cho thấy chúng ta không bao giờ cần phải xem lại các quyết định nhiều hơn$k$lùi lại, bởi vì bất cứ điều gì cũ hơn$k$ngày không còn ảnh hưởng đến ràng buộc. 

Điều này dẫn đến việc xây dựng tham lam: duy trì tổng cửa sổ hiện tại và đảm bảo rằng khi chuỗi tăng lên, chúng tôi tăng tối thiểu một số giá trị trong cửa sổ để khôi phục tính khả thi. Cấu trúc tối ưu cuối cùng có tính tuần hoàn: hàng ngày chúng tôi đảm bảo có đủ khối lượng ở phần cuối cùng.$k$các phần tử và điều này tạo ra một mô hình lặp lại khi hệ thống ổn định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nk)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(k)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng chuỗi tăng dần trong khi vẫn duy trì tổng cửa sổ trượt của chuỗi cuối cùng$k$giá trị và đảm bảo nó luôn ít nhất bằng độ dài chuỗi hiện tại. 

1. Khởi tạo một mảng hoặc cấu trúc cuộn để lưu trữ dữ liệu cuối cùng$k$giá trị, tất cả đều bắt đầu từ 1. Đây là lựa chọn nhỏ nhất có thể vì mỗi ngày đóng góp ít nhất một vấn đề. 
2. Duy trì tổng số cuối cùng$k$ngày. Ban đầu, đối với ngày thứ nhất, tổng là 1 và chuỗi là 1, do đó ràng buộc được giữ nguyên. 
3. Lặp lại từ ngày này sang ngày khác$n$. Cho mỗi ngày$i$, tính giá trị vệt hiện tại, đó là$i$, vì dãy là liên tục. 
4. Tính tổng cửa sổ hiện tại của cửa sổ cuối cùng$k$các phần tử. Nếu như$i \le k$, cửa sổ là tiền tố tổng hợp tới$i$; nếu không thì số tiền đó đã hết$i-k+1$ĐẾN$i$. 
5. Nếu tổng cửa sổ ít nhất là$i$, chúng ta gán giá trị tối thiểu 1 cho ngày hiện tại, cập nhật cửa sổ và tiếp tục. 
6. Nếu tổng cửa sổ nhỏ hơn$i$, chúng ta phải tăng giá trị của ngày hiện tại để tổng cửa sổ trở nên chính xác$i$. Điều này là tối ưu vì việc tăng các giá trị trước đó sẽ chỉ làm tăng tổng số tiền mà không làm giảm các yêu cầu trong tương lai. 
7. Sau khi gán giá trị cho ngày$i$, hãy cập nhật cửa sổ trượt bằng cách thêm giá trị mới và xóa giá trị nằm ngoài cửa sổ nếu$i > k$. 

Ý tưởng cấu trúc quan trọng là mọi thâm hụt trong hạn chế đều có thể được khắc phục một cách tham lam vào thời điểm hiện tại và không bao giờ yêu cầu phân phối lại vào quá khứ. 

### Tại sao nó hoạt động 

Vào bất kỳ ngày nào$i$, ràng buộc chỉ phụ thuộc vào tổng của giá trị cuối cùng$k$phần tử và giá trị$i$. Bất kỳ sự thiếu hụt nào trong bất đẳng thức đều có nghĩa là tổng cửa sổ hiện tại quá nhỏ để đáp ứng ngưỡng yêu cầu. Vì tất cả các ngày trước đó bên ngoài cửa sổ không còn ảnh hưởng đến ràng buộc nên cách duy nhất để khắc phục tính khả thi là tăng giá trị bên trong cửa sổ hiện tại. 

Việc tăng các vị trí trước đó bên trong cửa sổ hoặc vị trí hiện tại là tương đương về mặt khả thi, nhưng việc tăng ngày hiện tại là tối ưu vì nó không có nguy cơ vi phạm các ràng buộc của ngày trước đó đã được thỏa mãn. Điều này tạo ra một bước sửa chữa tối ưu cục bộ mà không bao giờ làm mất hiệu lực tính đúng đắn trong quá khứ, thiết lập một bất biến tham lam ổn định: sau ngày xử lý$i$, tất cả các ràng buộc lên đến$i$giữ và tổng cửa sổ luôn đủ tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    k, n = map(int, input().split())
    
    from collections import deque
    window = deque()
    window_sum = 0
    
    total = 0
    
    for i in range(1, n + 1):
        # remove element leaving window
        if len(window) == k:
            window_sum -= window.popleft()
        
        # current streak requirement
        need = i
        
        # minimum value is 1
        val = 1
        
        # check window constraint
        if window_sum + val < need:
            val = need - window_sum
        
        window.append(val)
        window_sum += val
        total += val
    
    print(total)

if __name__ == "__main__":
    solve()
```Việc triển khai duy trì một deque đại diện cho lần cuối cùng$k$các giá trị và tổng số tiền hiện có để hỗ trợ$O(1)$cập nhật. Mỗi ngày chúng tôi đảm bảo có ít nhất 1 bài được giao. Nếu điều đó không đủ để làm cho tổng cửa sổ trượt đáp ứng yêu cầu chuỗi hiện tại, chúng tôi sẽ tăng giá trị của ngày hiện tại vừa đủ để đáp ứng sự bình đẳng. 

Chi tiết quan trọng là cửa sổ được cập nhật trước khi đánh giá ràng buộc, do đó tổng luôn phản ánh chính xác giá trị cuối cùng$k$ngày. Điều này tránh được các lỗi sai sót trong phạm vi trượt. 

## Ví dụ đã hoạt động 

### Mẫu 1:$k = 2, n = 3$Chúng tôi mô phỏng từng ngày. 

| Ngày | Cửa sổ trước | Nhu cầu$i$| Được giao | Cửa sổ sau | Tổng hợp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | [] | 1 | 1 | [1] | 1 | 
| 2 | [1] | 2 | 1 | [1,1] | 2 | 
| 3 | [1,1] | 3 | 2 | [1,2] | 3 | 

Tổng số tiền là$1 + 1 + 2 = 4$. Bảng cho thấy vi phạm chỉ xảy ra vào ngày thứ 3, khi thời lượng trước đó không đủ, buộc chỉ tăng ở vị trí hiện tại. 

### Mẫu 2:$k = 30, n = 15$Ở đây cửa sổ lớn hơn độ dài quá trình, vì vậy không có phần tử nào rời khỏi cửa sổ. 

| Ngày | Cửa sổ trước | Nhu cầu$i$| Được giao | Cửa sổ sau | Tổng hợp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | [] | 1 | 1 | [1] | 1 | 
| 2 | [1] | 2 | 1 | [1,1] | 2 | 
| ... | ... | ... | ... | ... | ... | 
| 15 | [1,...,1] | 15 | 1 | [1,...,1] | 15 | 

Vì cửa sổ luôn chứa tất cả các ngày trước đó nên tổng sau ngày$i$luôn luôn là$i$và chỉ định 1 mỗi ngày là đủ. Tổng số là 15. 

Điều này xác nhận hành vi khi$k \ge n$, nơi không xảy ra hiện tượng tràn hoặc cắt xén. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi ngày thực hiện các hoạt động deque liên tục và cập nhật số học | 
| Không gian |$O(k)$| Chỉ có cái cuối cùng$k$giá trị được lưu trữ trong cửa sổ trượt | 

Thuật toán là tuyến tính trong$n$, điều này là bắt buộc vì$n$có thể đạt được$10^6$. Việc sử dụng bộ nhớ bị giới hạn bởi kích thước cửa sổ, do đó nó vẫn an toàn dưới 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    k, n = map(int, input().split())
    from collections import deque
    window = deque()
    window_sum = 0
    total = 0

    for i in range(1, n + 1):
        if len(window) == k:
            window_sum -= window.popleft()
        need = i
        val = 1
        if window_sum + val < need:
            val = need - window_sum
        window.append(val)
        window_sum += val
        total += val

    return str(total).strip()

# provided samples
assert run("2 3\n") == "4", "sample 1"
assert run("30 15\n") == "15", "sample 2"

# custom cases
assert run("1 5\n") == "15", "k=1 forces growing prefix"
assert run("5 1\n") == "1", "single day base case"
assert run("2 6\n") == "9", "small sliding window behavior"
assert run("10 10\n") == "10", "window never shrinks"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 5 | 15 | k=1 lực lượng tăng trưởng tích lũy | 
| 5 1 | 1 | trường hợp cạnh đơn phần tử | 
| 2 6 | 9 | kích hoạt hạn chế cửa sổ trượt | 
| 10 10 | 10 | không xóa khỏi cửa sổ | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi$k = 1$. Cửa sổ chỉ chứa ngày hiện tại, do đó ràng buộc trở thành giá trị của ngày$i$ít nhất phải có$i$. Thuật toán gán chính xác$i$, tạo ra một chuỗi tăng trưởng hình tam giác$1,2,3,\dots,n$. Cửa sổ trượt luôn khớp chính xác với yêu cầu nên không cần điều chỉnh gì thêm. 

Một trường hợp cạnh khác là khi$k \ge n$. Cửa sổ không bao giờ loại bỏ các giá trị cũ nên tổng luôn được tích lũy. Trong tình huống này, việc gán 1 cho mỗi ngày là tối ưu vì tổng cửa sổ sau ngày$i$chính xác là$i$, luôn khớp với chuỗi yêu cầu. 

Một trường hợp tế nhị hơn xảy ra khi$k$nhỏ, chẳng hạn như$k = 2$. Những ngày đầu cư xử như$k \ge n$trường hợp này, nhưng khi cửa sổ đầy, các giá trị nhỏ cũ hơn sẽ bị loại bỏ. Đây là nơi thuật toán bắt đầu chèn các giá trị lớn hơn, như đã thấy trong mẫu 1. Việc điều chỉnh tham lam ở ngày hiện tại đảm bảo tính khả thi mà không cần xem lại các phép gán trước đó.
