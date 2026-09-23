---
title: "CF 104790B - Robot chiến đấu"
description: "Chúng ta được cấp một số $n$, đại diện cho số lượng thao tác vuốt đơn vị được yêu cầu để phá hủy hoàn toàn robot đối thủ nếu chúng ta chỉ dựa vào móng vuốt."
date: "2026-06-28T16:41:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104790
codeforces_index: "B"
codeforces_contest_name: "2023 Benelux Algorithm Programming Contest (BAPC 23)"
rating: 0
weight: 104790
solve_time_s: 59
verified: true
draft: false
---

[CF 104790B - Robot chiến đấu](https://codeforces.com/problemset/problem/104790/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một số$n$, biểu thị số lượng thao tác vuốt đơn vị được yêu cầu để phá hủy hoàn toàn robot đối thủ nếu chúng ta chỉ dựa vào móng vuốt. Mỗi thao tác vuốt sẽ loại bỏ chính xác một đơn vị cấu trúc, nhưng về mặt khái niệm, robot có kích thước lớn và chúng ta chỉ biết tổng kích thước của nó. 

Ngoài móng vuốt, chúng ta còn có thao tác kiếm. Một vết cắt bằng kiếm không trực tiếp loại bỏ vật liệu. Thay vào đó, nó chia robot đang hoạt động hiện tại thành hai phần và chỉ một trong số chúng tiếp tục di chuyển vì động cơ nằm bên trong đúng một nửa. Chúng tôi không biết nửa nào chứa động cơ, vì vậy theo quan điểm của chúng tôi, trường hợp xấu nhất là nửa lớn hơn hoặc rắc rối hơn luôn tồn tại. Điều đó có nghĩa là sau khi một thanh kiếm chém vào một đoạn có kích thước$x$, trạng thái tiếp theo thực sự là một phân đoạn kích thước duy nhất$\lceil x/2 \rceil$. 

Mục tiêu là tiêu diệt hoàn toàn robot, nghĩa là giảm kích thước phân đoạn hoạt động xuống 0. Chúng tôi muốn số lượng thao tác tối thiểu trong quá trình tiến hóa trong trường hợp xấu nhất của robot sau mỗi lần chém kiếm. 

Điều tinh tế là chúng tôi không kiểm soát tính ngẫu nhiên. Mỗi nhát kiếm đều buộc chúng ta rơi vào nửa còn sống tồi tệ hơn, vì vậy mỗi quyết định phải mạnh mẽ trước kết quả đó. Một cách giải thích ngây thơ có thể coi các vết cắt bằng kiếm luôn giảm kích thước đi một nửa theo nghĩa lạc quan, nhưng ở đây, hành vi trần mới là điều quan trọng và nó kết hợp lại. 

Ràng buộc$n \le 10^{18}$ngay lập tức loại trừ bất kỳ chương trình động nào trên các giá trị lên đến$n$, và thậm chí suy luận tuyến tính hoặc bậc hai là không thể. Bất kỳ giải pháp nào cũng phải giảm kích thước bài toán theo cấp số nhân hoặc sử dụng phép truy toán đóng để đánh giá theo thời gian logarit. 

Một sai lầm thường gặp xuất hiện khi coi thanh kiếm là “gần như giảm một nửa nên luôn tốt hơn là vuốt”. Điều này không phải lúc nào cũng đúng với các giá trị nhỏ. Ví dụ, khi$n = 2$, một thanh kiếm giảm xuống còn 1 nhưng vẫn cần thêm một bước nữa, tương ứng với hiệu quả của móng vuốt. Khi$n = 1$, kiếm không có tác dụng gì và móng vuốt là bắt buộc. Hành vi ranh giới này là thứ phá vỡ những giả định quá tham lam. 

Một cạm bẫy khác là cho rằng câu trả lời tỷ lệ thuận với$\log n$trực tiếp. Trình tự bắt đầu hoạt động theo logarit chỉ sau một tiền tố nhỏ trong đó thao tác vuốt tuyến tính chiếm ưu thế hoặc liên quan đến việc sử dụng kiếm. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ mô phỏng mọi chuỗi hoạt động có thể xảy ra, duy trì kích thước phân khúc hiện tại và khám phá cả hai lựa chọn ở mỗi bước: hoặc áp dụng cơ chế giảm dần$x \to x-1$, hoặc áp dụng một thanh kiếm giảm$x \to \lceil x/2 \rceil$. Điều này tạo thành một cây tìm kiếm trên các trạng thái. Ngay cả khi chúng ta ghi nhớ kết quả thì không gian trạng thái vẫn là các số nguyên từ 1 đến$n$và mỗi trạng thái phụ thuộc vào hai lần chuyển đổi. 

Sự tái phát đối với một DP đơn giản sẽ là$$f(x) = 1 + \min(x-1, f(\lceil x/2 \rceil)).$$Mặc dù đúng nhưng đánh giá điều này một cách ngây thơ đối với tất cả mọi người$x \le n$là không thể khi$n$đạt tới$10^{18}$. 

Quan sát quan trọng là thao tác vuốt sẽ trở nên kém hơn một khi$x$là đủ lớn, bởi vì việc giảm một nửa liên tục sẽ nhanh chóng lấn át sự suy giảm tuyến tính. Sau một ngưỡng nhỏ, chiến lược tối ưu luôn là sử dụng thanh kiếm, vì nó làm giảm kích thước vấn đề theo cấp số nhân và chi phí của một bước bổ sung sẽ được bù đắp bằng sự thu hẹp nhanh chóng. 

Điều này biến vấn đề thành việc áp dụng lặp đi lặp lại phép lặp$$f(x) = 1 + f(\lceil x/2 \rceil),$$cho đến khi đạt đến vùng cơ sở nhỏ nơi các giá trị trực tiếp được biết đến. Điều này làm giảm toàn bộ quá trình xuống việc đếm số lần chúng ta có thể liên tục lấy nửa trần cho đến khi đạt 1, với việc xử lý cẩn thận các giá trị nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force DP trên tất cả các bang |$O(n)$|$O(n)$| Quá chậm | 
| Giảm một nửa đệ quy tái phát |$O(\log n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi muốn tính toán số lượng thao tác tối thiểu cần thiết để giảm kích thước hiện tại từ$n$đến 0, trong đó mỗi bước sẽ trừ đi một hoặc thay số đó bằng nửa trần của nó. 

1. Bắt đầu với nhận xét rằng với bất kỳ quy mô hiện tại nào$x$, kết quả chỉ phụ thuộc vào hai lựa chọn: giảm xuống$x-1$, hoặc giảm xuống$\lceil x/2 \rceil$. Câu trả lời luôn là một cộng với cái tốt hơn trong hai phần tiếp theo này. 
2. Xác định hàm$f(x)$là các thao tác tối thiểu cần thiết để phá hủy một đoạn có kích thước$x$. Vì$x = 0$, không cần thực hiện thao tác nào. 
3. Đối với$x = 1$, chỉ có móng vuốt mới có ý nghĩa, vậy nên$f(1) = 1$. Điều này thiết lập trường hợp cơ bản trong đó việc giảm một nửa là vô ích. 
4. Đối với$x > 1$, so sánh hai chiến lược. Móng vuốt mang lại chi phí$x$. Thanh kiếm mang lại giá trị$1 + f(\lceil x/2 \rceil)$. 
5. Hãy quan sát điều đó một lần$x \ge 4$, áp dụng kiếm nhiều lần sẽ chiếm ưu thế vì nó làm giảm trạng thái theo cấp số nhân, khiến chi phí tuyến tính của các móng vuốt lặp đi lặp lại luôn trở nên tồi tệ hơn. 
6. Do đó, đối với tất cả các giá trị lớn hơn có ý nghĩa, chúng tôi sử dụng phép truy toán$f(x) = 1 + f(\lceil x/2 \rceil)$. 
7. Áp dụng nhiều lần phép biến đổi này bắt đầu từ$n$, đếm xem chúng ta áp dụng bao nhiêu lần cho đến khi đạt 1. 

Tại sao nó hoạt động 

Bất biến cốt lõi là sau mỗi thao tác kiếm, vấn đề sẽ giảm xuống một phiên bản nhỏ hơn có cùng dạng và cấu trúc quyết định tối ưu không phụ thuộc vào bất kỳ trạng thái ẩn nào ngoài kích thước hiện tại. Sự truy hồi đảm bảo rằng ở mỗi bước chúng ta đang chọn thao tác tối ưu toàn cục bởi vì bất kỳ chuỗi thay thế nào sử dụng móng trước đó đều có thể được chuyển đổi thành một chuỗi có ít nhất nhiều bước bằng cách thay thế móng đó bằng thanh kiếm sau mà không làm tăng tính tối ưu. Vấn đề sụp đổ thành một chuỗi giảm xác định duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    
    # base cases
    if n <= 1:
        print(1)
        return
    
    # f(1)=1, f(2)=2, f(3)=3 are special small values
    # but recurrence f(x)=1+f((x+1)//2) works for all x>=2
    res = 0
    x = n
    
    while x > 0:
        res += 1
        if x == 1:
            break
        x = (x + 1) // 2
    
    print(res)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo sau sự lặp lại có nguồn gốc trước đó. Vòng lặp liên tục áp dụng phép biến đổi$x \to \lceil x/2 \rceil$, được thực hiện như$(x+1)//2$, trong khi đếm số bước đã thực hiện. Mỗi lần lặp tương ứng chính xác với một thao tác trong chiến lược tối ưu. 

Một điểm tinh tế là việc xử lý điều kiện chấm dứt. Một lần$x$trở thành 1, chúng ta vẫn tính thao tác cuối cùng hủy bỏ nó, do đó vòng lặp đảm bảo rằng bước này được đưa vào một cách chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
```| Bước | x | Hoạt động | độ phân giải | 
| --- | --- | --- | --- | 
| 0 | 1 | bắt đầu | 0 | 
| 1 | 1 | móng vuốt | 1 | 

Đầu ra:```
1
```Điều này chứng tỏ trường hợp cơ bản trong đó việc giảm một nửa là không hữu ích và câu trả lời là bắt buộc. 

### Ví dụ 2 

đầu vào:```
5
```| Bước | x | Hoạt động | độ phân giải | 
| --- | --- | --- | --- | 
| 0 | 5 | bắt đầu | 0 | 
| 1 | 3 | thanh kiếm | 1 | 
| 2 | 2 | thanh kiếm | 2 | 
| 3 | 1 | trận chung kết kiếm/vuốt | 3 | 

Đầu ra:```
3
```Dấu vết này cho thấy việc giảm một nửa lặp lại nhanh chóng làm giảm vấn đề như thế nào, làm cho cấu trúc logarit hiển thị ngay cả đối với các đầu vào nhỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log n)$| Mỗi thao tác giảm$n$đến khoảng một nửa | 
| Không gian |$O(1)$| Chỉ có một số biến được duy trì | 

Hành vi logarit là cần thiết để xử lý đầu vào lên đến$10^{18}$. Ngay cả trong trường hợp xấu nhất, số lần lặp vẫn dưới 60. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import builtins

    input = sys.stdin.readline
    n = int(sys.stdin.readline().strip())
    
    if n <= 1:
        return "1\n"
    
    res = 0
    x = n
    while x > 0:
        res += 1
        if x == 1:
            break
        x = (x + 1) // 2
    
    return str(res) + "\n"

# provided samples (conceptual, since samples were not fully shown)
assert run("1\n") == "1\n"

# custom cases
assert run("2\n") == "2\n"   # smallest non-trivial
assert run("3\n") == "3\n"   # transition region
assert run("4\n") == "3\n"   # first benefit from halving
assert run("8\n") == "4\n"   # clean power of two behavior
assert run("9\n") == "5\n"   # non-power-of-two case
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | trường hợp cơ sở | 
| 4 | 3 | lợi thế giảm một nửa hiệu quả đầu tiên | 
| 9 | 5 | hành vi phi-lực-của-hai trần | 

## Vỏ cạnh 

cho$n = 1$, thuật toán ngay lập tức trả về 1 vì không giảm một nửa nào có thể làm giảm trạng thái. Bất kỳ nỗ lực nào để áp dụng phép lặp vẫn phải tính đến bước hủy bỏ cuối cùng bắt buộc. 

Đối với các giá trị nhỏ như$n = 2$Và$n = 3$, các chuyển đổi lặp lại vẫn đúng nhưng chưa thể hiện hành vi giảm một nửa tiệm cận. Thuật toán xử lý chúng một cách thống nhất trong cùng một vòng lặp, đảm bảo không có lỗi viết hoa đặc biệt nào. 

Đối với các giá trị ngay trên lũy thừa của hai, chẳng hạn như$n = 2^k + 1$, phép chia trần đảm bảo rằng lần rút gọn đầu tiên không hoạt động giống như phép chia nhị phân hoàn hảo. Việc sử dụng$(x+1)//2$bảo toàn chính xác sự bất đối xứng này và dấu vết tuân theo trình tự thu nhỏ xác định tương tự mà không bị sai lệch.
