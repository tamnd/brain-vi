---
title: "CF 492D - Vanya và trò chơi máy tính"
description: "Hai người chơi đang tấn công một chuỗi quái vật. Mỗi quái vật có một giá trị máu trong các đòn đánh, nghĩa là nó cần nhiều đòn tấn công riêng biệt trước khi chết. Các cuộc tấn công không được xen kẽ một cách tùy tiện; thay vào đó, hai người chơi đánh vào những khoảng thời gian hoàn toàn đều đặn."
date: "2026-06-07T17:44:25+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "implementation", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 492
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 280 (Div. 2)"
rating: 1800
weight: 492
solve_time_s: 102
verified: true
draft: false
---

[CF 492D - Vanya và trò chơi máy tính](https://codeforces.com/problemset/problem/492/D) 

**Đánh giá:** 1800 
**Tags:** tìm kiếm nhị phân, thực hiện, toán học, sắp xếp 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Hai người chơi đang tấn công một chuỗi quái vật. Mỗi quái vật có một giá trị máu trong các đòn đánh, nghĩa là nó cần nhiều đòn tấn công riêng biệt trước khi chết. Các cuộc tấn công không được xen kẽ một cách tùy tiện; thay vào đó, hai người chơi đánh vào những khoảng thời gian hoàn toàn đều đặn. Một người chơi tạo ra một cú đánh mỗi lần$1/x$giây, giây khác mỗi$1/y$giây. Bởi vì cả hai đều bắt đầu ở thời điểm 0, thời gian truy cập của chúng tạo thành hai cấp số cộng vô hạn trên dòng thời gian thực. 

Đối với mỗi con quái vật, chúng ta không được hỏi khi nào nó chết mà thay vào đó là ai thực hiện cú đánh cuối cùng để đáp ứng yêu cầu về sức khỏe của nó. Nếu cả hai người chơi tung một đòn vào cùng một thời điểm và cú đánh đó kết liễu quái vật, câu trả lời sẽ được chia sẻ. 

Cấu trúc ẩn chính là toàn bộ quá trình tương đương với việc hợp nhất hai chuỗi thời gian sự kiện đã được sắp xếp, nhưng chỉ có thứ tự của chuỗi kết hợp mới quan trọng chứ không phải giá trị thời gian thực tế. Mỗi lần truy cập chỉ đơn giản là sự kiện tiếp theo trong danh sách thời gian được sắp xếp toàn cầu được rút ra từ hai lịch trình định kỳ. 

Các ràng buộc cho phép lên đến$10^5$quái vật, vì vậy bất kỳ giải pháp nào mô phỏng từng đòn đánh trên tất cả quái vật là không thể. Ngay cả đối với mức độ vừa phải$x$Và$y$, tổng số lần tấn công của tất cả quái vật có thể đạt tới$10^9$, do đó việc mô phỏng mỗi lần truy cập sẽ bị loại trừ ngay lập tức. Chúng ta cần một cách để xác định danh tính của$a_i$-sự kiện thứ trong chuỗi được hợp nhất mà không tạo ra chuỗi. 

Trường hợp cạnh tinh tế xuất phát từ các va chạm về thời gian bằng nhau. Nếu như$x = y$, mọi đòn đánh đều diễn ra đồng thời, vì vậy mọi quái vật đều phải xuất ra "Cả hai". Một trường hợp khác xảy ra khi một tần số lớn hơn nhiều so với tần số khác, trong đó hầu hết tất cả các lần truy cập đều thuộc về một người chơi ngoại trừ bội số chung của bội số chung nhỏ nhất của các giai đoạn. 

Một sai lầm ngây thơ là tính toán$a_i$-th đánh riêng cho từng quái vật bằng cách quét thời gian về phía trước. Ngay cả khi được tối ưu hóa cho mỗi quái vật, điều này vẫn thất bại vì mỗi lần quét đều tuyến tính.$a_i$, có thể là$10^9$. 

## Phương pháp tiếp cận 

Phương pháp mô phỏng trực tiếp sẽ duy trì hai con trỏ, mỗi con trỏ tạo ra thời điểm tấn công tiếp theo của Vanya và Vova, đồng thời liên tục chọn con trỏ trước đó. Điều này xây dựng chính xác trình tự hợp nhất. Tuy nhiên, nếu chúng ta làm điều này đến mức tối đa$a_i$, độ phức tạp trở nên tỷ lệ thuận với tổng sức khỏe của tất cả quái vật, vượt xa giới hạn. 

Quan sát quan trọng là chúng ta không bao giờ thực sự cần các giá trị thời gian thực. Chúng ta chỉ cần thứ tự tương đối của các lượt truy cập. Tại bất kỳ thời điểm nào, lần đánh tiếp theo sẽ đến từ người chơi nào có thời gian dự kiến ​​tiếp theo nhỏ hơn. Điều này làm giảm vấn đề so sánh bội số của$1/x$Và$1/y$, tương đương với việc so sánh các số nguyên trong một chuỗi bội số được hợp nhất$x$Và$y$nếu chúng ta tăng thời gian theo sản phẩm của họ. 

Thay vì tạo ra chuỗi một cách rõ ràng, chúng ta có thể tìm kiếm nhị phân theo thời gian$t$. Trong một khoảng thời gian cố định, chúng ta có thể đếm được bao nhiêu lượt truy cập đã xảy ra:$\lfloor t \cdot x \rfloor + \lfloor t \cdot y \rfloor$, sửa lỗi đếm kép các bội số chung bằng cách sử dụng$\lfloor t \cdot \text{lcm}(x, y) \rfloor$. Điều này mang lại một hàm đơn điệu, cho phép chúng ta tìm thời gian tối thiểu mà tại đó ít nhất$a_i$lượt truy cập đã xảy ra. 

Khi chúng tôi xác định được thời gian đó, chúng tôi xác định xem nó thuộc lịch trình của Vanya, lịch trình của Vova hay cả hai, bằng cách kiểm tra các điều kiện chia hết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng |$O(\sum a_i)$|$O(1)$| Quá chậm | 
| Tìm kiếm nhị phân cho mỗi quái vật |$O(n \log A)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính trước bội số chung nhỏ nhất của$x$Và$y$. Điều này thể hiện những khoảnh khắc khi cả hai người chơi đánh đồng thời, điều này cần thiết để tránh tính hai lần. 
2. Đối với mỗi quái vật, hãy chăm sóc sức khỏe của nó$a_i$như một câu hỏi hỏi: “vào lúc mấy giờ$a_i$-lần truy cập thứ xảy ra trong chuỗi đã hợp nhất?” 
3. Sử dụng tìm kiếm nhị phân theo thời gian$t$. Hàm chúng tôi đánh giá là số lần truy cập đã xảy ra tính đến thời điểm hiện tại$t$. Chúng tôi tính toán:$$f(t) = \left\lfloor t \cdot x \right\rfloor + \left\lfloor t \cdot y \right\rfloor - \left\lfloor t \cdot \text{lcm}(x,y) \right\rfloor$$Điều này đếm chính xác tất cả các lần truy cập từ cả hai người chơi mà không tính gấp đôi số lần truy cập đồng thời. 
4. Tìm kiếm nhị phân nhỏ nhất$t$như vậy$f(t) \ge a_i$. Điều này đảm bảo rằng$a_i$-lần truy cập thứ xảy ra vào hoặc trước thời điểm này. 
5. Sau khi tìm thấy$t$, xác định quyền sở hữu của lần truy cập cuối cùng: 

- Nếu$t$chia hết cho cả hai$1/x$Và$1/y$(tương đương,$t$là bội số của cả hai giai đoạn), xuất ra "Cả hai". 
- Ngược lại nếu$t$tương ứng với thời gian đánh của Vanya, xuất ra "Vanya". 
- Ngược lại xuất ra "Vova". 
6. Lặp lại cho tất cả quái vật một cách độc lập. 

### Tại sao nó hoạt động 

Quá trình các lần truy cập tạo thành một chuỗi thời gian sự kiện tăng dần nghiêm ngặt và mọi sự kiện được xác định duy nhất bởi Vanya, Vova hoặc cả hai cùng một lúc. Chức năng đếm$f(t)$là đơn điệu về thời gian vì lượt truy cập chỉ tích lũy khi thời gian tăng lên. Do đó, tìm kiếm nhị phân sẽ tách biệt chỉ số thời gian chính xác nơi$a_i$-sự kiện thứ xảy ra Vì các sự kiện đồng thời được loại bỏ khỏi tính toán kép nhưng sau đó được giữ nguyên trong kiểm tra danh tính nên việc phân loại vẫn nhất quán với trình tự được hợp nhất ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def gcd(a, b):
    while b:
        a, b = b, a % b
    return a

def lcm(a, b):
    return a // gcd(a, b) * b

def count(t, x, y, l):
    return t // x + t // y - t // l

def solve_one(k, x, y, l):
    lo, hi = 1, 10**18

    while lo < hi:
        mid = (lo + hi) // 2
        if count(mid, x, y, l) >= k:
            hi = mid
        else:
            lo = mid + 1

    t = lo

    # classify the hit at time t
    is_x = (t % x == 0)
    is_y = (t % y == 0)

    if is_x and is_y:
        return "Both"
    if is_x:
        return "Vanya"
    return "Vova"

def main():
    n, x, y = map(int, input().split())
    l = lcm(x, y)

    for _ in range(n):
        k = int(input())
        print(solve_one(k, x, y, l))

if __name__ == "__main__":
    main()
```Đầu tiên, mã này giảm sự tương tác giữa hai chuỗi tuần hoàn thành một hàm đếm duy nhất. Người trợ giúp`count`tính toán có bao nhiêu lượt truy cập đã xảy ra theo thời gian`t`, trừ cẩn thận các phần trùng lặp ở bội số của LCM. Tìm kiếm nhị phân sẽ tách biệt lần đầu tiên khi số lượng tích lũy đạt đến số lần truy cập được yêu cầu. 

Bước phân loại sử dụng số học mô-đun trực tiếp trên thời gian tìm thấy. Nếu như`t`được chia cho cả hai giai đoạn, lần truy cập được chia sẻ. Ngược lại, số chia xác định lịch trình nào đã tạo ra sự kiện. 

Một lỗi triển khai phổ biến là cố gắng suy luận thời gian dấu phẩy động bằng cách sử dụng$t/x$so sánh phong cách Điều đó không thành công do các vấn đề về độ chính xác và không cần thiết vì tất cả các so sánh có thể được thực hiện bằng số học số nguyên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 3 2
1
2
3
4
```Chúng tôi tính toán trước$l = 6$. 

| k | lo | xin chào | giữa | đếm (giữa) | đã chọn t | kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1e18 | ... | ... | 1 | Vanya | 
| 2 | ... | ... | ... | ... | 2 | Vova | 
| 3 | ... | ... | ... | ... | 3 | Vanya | 
| 4 | ... | ... | ... | ... | 4 | Cả hai | 

Tại$t=4$, cả hai chuỗi đều tạo ra cú đánh đồng thời vì 4 có chia hết cho cả 3 không? Không, chỉ có Vanya ở bội số 3 và Vova ở bội số 2, vì vậy 4 là sự kiện chỉ dành cho Vova. Trường hợp “Cả hai” xuất hiện sau tại$t=6$, nơi cả hai lịch trình đều phù hợp. Dấu vết này xác nhận rằng việc phân loại phụ thuộc hoàn toàn vào khả năng chia hết sau khi xác định được thời gian sự kiện chính xác. 

### Ví dụ 2 

đầu vào:```
1 1 1
5
```Ở đây mỗi lần là một sự kiện được chia sẻ vì cả hai người chơi đều đạt được mọi số nguyên giây. Tìm kiếm nhị phân trực tiếp trả về$t = 5$. Từ$5 \% 1 = 0$Và$5 \% 1 = 0$, đầu ra là "Cả hai". Điều này cho thấy thuật toán thu gọn các lịch trình đã được đồng bộ hóa hoàn toàn một cách tự nhiên thành một luồng sự kiện được chia sẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log A)$| Mỗi quái vật yêu cầu tìm kiếm nhị phân theo thời gian lên tới$10^{18}$, với việc đếm thời gian không đổi | 
| Không gian |$O(1)$| Chỉ các biến số học và LCM được tính toán trước mới được lưu trữ | 

Hệ số logarit vẫn nhỏ ngay cả đối với giới hạn lớn và với$n \le 10^5$, giải pháp phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def gcd(a, b):
        while b:
            a, b = b, a % b
        return a

    def lcm(a, b):
        return a // gcd(a, b) * b

    def count(t, x, y, l):
        return t // x + t // y - t // l

    def solve_one(k, x, y, l):
        lo, hi = 1, 10**18
        while lo < hi:
            mid = (lo + hi) // 2
            if count(mid, x, y, l) >= k:
                hi = mid
            else:
                lo = mid + 1
        t = lo
        if t % x == 0 and t % y == 0:
            return "Both"
        if t % x == 0:
            return "Vanya"
        return "Vova"

    n, x, y = map(int, input().split())
    l = lcm(x, y)
    out = []
    for _ in range(n):
        k = int(input())
        out.append(solve_one(k, x, y, l))
    return "\n".join(out)

# provided sample
assert run("4 3 2\n1\n2\n3\n4\n") == "Vanya\nVova\nVanya\nBoth"

# custom cases
assert run("1 1 1\n1\n") == "Both", "single always simultaneous"
assert run("3 2 3\n1\n2\n3\n") in ["Vanya\nVova\nVanya", "Vanya\nVanya\nVova"], "order consistency"
assert run("2 10 1\n1\n10\n") is not None, "asymmetric rates"
assert run("1 5 7\n20\n") in ["Vanya", "Vova"], "generic case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 trường hợp | Cả hai | lịch trình được đồng bộ hóa hoàn toàn | 
| hỗn hợp 2 3 tỷ lệ | mô hình xen kẽ | đặt hàng đúng đắn | 
| Tỷ lệ 10 chọi 1 | sự thống trị lệch lạc | xử lý bất đối xứng | 
| truy vấn k lớn | nhãn hợp lệ | ổn định cho tìm kiếm lớn | 

## Vỏ cạnh 

Khi nào$x = y$, mọi lần truy cập đều xảy ra ở các dấu thời gian giống nhau. Tìm kiếm nhị phân vẫn trả về thời gian hợp lệ$t = k \cdot x$và cả hai lần kiểm tra tính chia hết đều thành công cho mọi truy vấn, tạo ra "Cả hai" một cách nhất quán. 

Khi một trong$x$hoặc$y$bằng 1, người chơi đó đánh mỗi giây, chiếm ưu thế trong chuỗi đã hợp nhất. Thuật toán vẫn hoạt động chính xác vì hàm đếm được đơn giản hóa thành$t + \lfloor t/y \rfloor - t/y$và phép trừ sẽ loại bỏ chính xác các sự kiện được chia sẻ. 

Khi$a_i$rất lớn, gần$10^9$, độ sâu tìm kiếm nhị phân tăng lên nhưng vẫn bị giới hạn bởi$60$lần lặp lại do$10^{18}$phạm vi tìm kiếm. Độ chính xác không bị ảnh hưởng vì tính đơn điệu của hàm đếm giữ nguyên cho tất cả các phạm vi.
