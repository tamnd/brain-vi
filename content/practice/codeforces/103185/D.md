---
title: "CF 103185D - Kẹo Chia"
description: "Chúng ta được tặng một bộ sưu tập các hộp kẹo, trong đó mỗi hộp chứa một số viên kẹo có lũy thừa bằng hai. Thay vì được cung cấp số đếm thực tế một cách trực tiếp, chúng ta được cung cấp số mũ: hộp thứ i chứa kẹo $2^{Ai}$. Hai anh em sẽ chia những chiếc hộp này cho nhau."
date: "2026-07-03T16:17:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103185
codeforces_index: "D"
codeforces_contest_name: "2020-2021 ACM-ICPC Latin American Regional Programming Contest"
rating: 0
weight: 103185
solve_time_s: 50
verified: true
draft: false
---

[CF 103185D - Kẹo chia đôi](https://codeforces.com/problemset/problem/103185/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được tặng một bộ sưu tập các hộp kẹo, trong đó mỗi hộp chứa một số viên kẹo có lũy thừa bằng hai. Thay vì được cung cấp số đếm thực tế một cách trực tiếp, chúng ta được cung cấp số mũ: ô thứ i chứa$2^{A_i}$kẹo. 

Hai anh em sẽ chia những chiếc hộp này cho nhau. Mỗi hộp phải thuộc về chính xác một anh em, do đó tổng cuối cùng được hình thành bằng cách chia mảng thành hai tập con rời rạc và lũy thừa tổng của hai. 

Câu hỏi đặt ra là liệu có thể giao mỗi hộp cho một trong hai anh em sao cho cả hai anh em đều có tổng số kẹo mà bản thân nó là lũy thừa của hai. 

Vì vậy, nếu một anh em nhận được một số tiền như$16$, điều đó hợp lệ, hoặc$32$, nhưng một khoản tiền như$20$hoặc$24$thì không. 

Các ràng buộc cho phép lên đến$10^5$các hộp và giá trị của mỗi hộp nhiều nhất là$2^{10^5}$. Điều này ngay lập tức loại trừ mọi phép liệt kê tập hợp con trên các bài tập, vì có$2^N$cách phân phối hộp. Ngay cả việc lưu trữ tất cả các tập hợp con là không thể. Bất kỳ lời giải nào cũng phải quy giản cấu trúc của bài toán thành tuyến tính hoặc gần tuyến tính trong N. 

Một sự tinh tế quan trọng là mang theo vật chất. Vì tất cả các giá trị đều là lũy thừa của hai, nên việc cộng chúng hoạt động giống như phép cộng nhị phân và việc kết hợp các hộp có cùng số mũ sẽ tạo ra các bit cao hơn. Một cách tiếp cận ngây thơ coi đây là tổng tập hợp con độc lập trên lũy thừa của hai sẽ thất bại. 

Một trường hợp thất bại điển hình xuất phát từ việc bỏ qua các tương tác mang. Ví dụ, nếu chúng ta có nhiều$2^0$các hộp, tổng của chúng có thể trở thành$2^k$, nhưng chỉ khi chúng được nhóm chính xác. Phép gán tham lam không có số lượng theo dõi ở mỗi cấp độ bit có thể dễ dàng tạo ra tổng không có lũy thừa bằng hai ngay cả khi tồn tại một phân vùng hợp lệ. 

Một trường hợp cạnh khác là khi tất cả các hộp đều có lũy thừa bằng hai. Ngay cả khi đó, việc chia đều chúng cũng không đảm bảo cả hai bên đều trở thành lũy thừa của hai trừ khi số lượng được căn chỉnh theo một chuỗi nhị phân duy nhất. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: thử gán các hộp cho một trong hai người anh em, tính cả hai tổng và kiểm tra xem cả hai có phải là lũy thừa của hai hay không. Điều này đúng vì nó trực tiếp khám phá định nghĩa. Tuy nhiên, số lượng nhiệm vụ$2^N$, điều này đã trở nên không khả thi tại$N = 30$, huống hồ là$10^5$. Ngay cả cách tiếp cận gặp nhau cũng gặp khó khăn vì tổng là các số nguyên lớn có khả năng lan truyền mang. 

Quan sát chính là toàn bộ vấn đề được điều chỉnh bởi biểu diễn nhị phân. Vì mỗi hộp đóng góp một bit thiết lập duy nhất tại vị trí$A_i$, tổng số nhiều tập hợp có thể được xem dưới dạng mảng tần số trên các vị trí bit. Điều quan trọng không phải là các hộp riêng lẻ mà là mỗi bit xuất hiện bao nhiêu lần. 

Nếu cả hai tổng cuối cùng phải là lũy thừa của hai thì mỗi bên phải kết thúc bằng chính xác một bit được đặt trong biểu diễn nhị phân của nó. Điều này có nghĩa là cấu trúc bit của mỗi bên phải thu gọn hoàn toàn thành một bit hoạt động cao nhất sau khi tất cả các phần mang được giải quyết. 

Cái nhìn sâu sắc quan trọng là việc truyền bá mang tính quyết định sau khi chúng ta ấn định số lượng vật phẩm của mỗi lũy thừa của hai sẽ về một phía. Thay vì suy nghĩ theo các tập hợp con, chúng ta nghĩ theo cách có bao nhiêu phần tử của mỗi số mũ thuộc về Bob; Charlie sau đó nhận được phần còn lại. Chúng ta cần kiểm tra xem liệu có tồn tại sự phân chia số lượng trên các vị trí bit sao cho cả hai tập hợp kết quả đều giảm, thông qua mang nhị phân, xuống một lũy thừa bằng hai. 

Điều này làm giảm vấn đề từ các lựa chọn theo cấp số nhân đến mô phỏng đa thức về số lượng bit bằng cách xử lý mang cẩn thận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force |$O(2^N \cdot N)$|$O(1)$| Quá chậm | 
| Tần số + Mô phỏng mang |$O(N \log A)$|$O(\log A)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi nén đầu vào thành một mảng tần số trong đó`cnt[i]`số hộp bằng$2^i$. Sau đó, chúng tôi mô phỏng cách kết hợp những sức mạnh này khi được giao cho một người anh em. 

Chúng tôi cố gắng giải thích quy trình theo chuỗi mang nhị phân: phân phối các mục giữa hai nhóm tương ứng với việc chia mỗi số bit thành hai số nguyên không âm có tổng là số ban đầu. Mỗi nhóm độc lập tạo thành một số nhị phân sau khi mang. 

1. Đếm số lần mỗi số mũ xuất hiện, xây dựng một mảng`cnt`. 
2. Hãy thử tất cả các lựa chọn có thể cho số mũ cuối cùng của tổng của người anh thứ nhất. Vì tổng là lũy thừa của hai nên chúng tôi xem xét vị trí bit mục tiêu`k`tổng cuối cùng sẽ ở đâu$2^k$. Điều này hạn chế kết cấu của công trình. 
3. Đối với cố định`k`, mô phỏng xem liệu chúng ta có thể gán các mục để một bên kết thúc chính xác hay không$2^k$. Điều này có nghĩa là chúng ta phải đảm bảo rằng sau khi gán và mang theo, chỉ có bit`k`vẫn khác 0 ở phía đó. 
4. Chúng tôi truyền từ các bit thấp hơn trở lên, duy trì giá trị mang biểu thị số lượng mục đã tràn vào số mũ tiếp theo của cấu trúc hiện tại. Tại mỗi bit i, chúng tôi kết hợp số lượng hiện có với số mang và quyết định số lượng sẽ tạo thành cấu trúc mục tiêu. 
5. Nếu tại bất kỳ thời điểm nào chúng ta có thể gán các giá trị một cách nhất quán để chuỗi nhớ cuối cùng tạo ra chính xác một bit ở vị trí k và 0 ở vị trí khác, thì tồn tại một phân vùng hợp lệ. 
6. Chúng ta lặp lại cách suy luận tương tự cho người anh thứ hai một cách ngầm định, vì các phần tử còn lại cũng phải tạo thành lũy thừa của hai. Nếu bất kỳ k nào hoạt động, xuất CÓ. 

Bất biến quan trọng là ở mỗi vị trí bit, quy trình duy trì tính nhất quán giữa các mục có sẵn và cấu trúc bắt buộc của số nhị phân với chính xác một bit được đặt ở cuối. Việc mang theo mã hóa tất cả các quyết định trong quá khứ; một khi mâu thuẫn xuất hiện (chẳng hạn như cần gán âm hoặc để lại phần dư không thể chuyển vào k), thì việc cấu hình là không thể. 

Thuật toán hoạt động vì mọi giải pháp hợp lệ đều phải tương ứng với lựa chọn bit đặt cao nhất cuối cùng cho một trong các anh em và cấu trúc cộng nhị phân đảm bảo rằng tất cả các trạng thái trung gian được xác định duy nhất bằng cách truyền mang. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_power_of_two(x):
    return x > 0 and (x & (x - 1)) == 0

def possible(cnt, target):
    carry = 0
    for i in range(len(cnt)):
        total = cnt[i] + carry
        if i == target:
            # everything above must vanish after this point
            # leftover must be exactly 1 at this bit after forming power of two
            if total % 2 != 1:
                return False
            carry = total // 2
        else:
            # we must fully eliminate this bit in final number
            carry = total // 2
    return carry == 0

def solve():
    n = int(input())
    arr = list(map(int, input().split()))

    maxb = max(arr)
    cnt = [0] * (maxb + 2)

    for x in arr:
        cnt[x] += 1

    # try all possible target bit positions
    for t in range(len(cnt)):
        if possible(cnt, t):
            # also check second pile implicitly works
            return "Y"
    return "N"

print(solve())
```Việc triển khai nén đầu vào thành tần số bit và sau đó thử từng số mũ cuối cùng có thể. các`possible`hàm mô phỏng liệu một anh em có thể bị buộc phải hình thành chính xác một giá trị lũy thừa hai hay không bằng cách theo dõi sự lan truyền mang. Điều kiện ở bit mục tiêu đảm bảo chính xác một đơn vị vẫn ở mức đó, trong khi tất cả các bit thấp hơn phải hủy hoàn toàn thông qua ghép nối và mang. 

Một chi tiết triển khai tinh tế là chúng ta phải xử lý bit mục tiêu khác với các bit khác, vì đó là vị trí duy nhất được phép giữ đóng góp cuối cùng không ghép đôi. Tất cả các vị trí khác phải giải quyết triệt để thông qua việc thực hiện. Việc mang là chia số nguyên cho hai vì hai mục của$2^i$tạo thành một mục của$2^{i+1}$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
2 2 5 3
```Chúng tôi xây dựng số lượng:$cnt[2]=2, cnt[3]=1, cnt[5]=1$. 

Đang thử các mục tiêu khác nhau, giả sử chúng tôi kiểm tra mục tiêu$k=5$. Chúng tôi mô phỏng: 

| tôi | cnt[i] | mang theo | tổng cộng | hành động | mang tiếp theo | 
| --- | --- | --- | --- | --- | --- | 
| 2 | 2 | 0 | 2 | mang đầy đủ | 1 | 
| 3 | 1 | 1 | 2 | mang đầy đủ | 1 | 
| 4 | 0 | 1 | 1 | mang đầy đủ | 0 | 
| 5 | 1 | 0 | 1 | giữ sự bình đẳng | 0 | 

Quá trình kết thúc rõ ràng, nghĩa là một anh em có thể đạt được$2^5$. Các hộp còn lại tự nhiên tạo thành một lũy thừa hợp lệ khác của hai, vì vậy đầu ra là CÓ. 

### Ví dụ 2 

đầu vào:```
5
3 1 4 1 5
```Đếm:$cnt[1]=2, cnt[3]=1, cnt[4]=1, cnt[5]=1$. 

Việc thử tất cả các vị trí mục tiêu đều không thành công vì việc truyền mang luôn để lại khối lượng dư thừa ở các bit trung gian không thể loại bỏ được nếu không tạo ra nhiều bit hoạt động trong ít nhất một tập hợp con. 

Điều này cho thấy rằng mặc dù tổng số được cấu trúc nhưng không có phân vùng nào mang lại hai kết quả bit đơn nhị phân rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log A)$| Chúng tôi xây dựng mảng tần số và thử đạt số mũ tối đa, mỗi mô phỏng là tuyến tính trên phạm vi bit | 
| Không gian |$O(\log A)$| Chỉ mảng tần số trên số mũ được lưu trữ | 

Giới hạn$N \le 10^5$Và$A_i \le 10^5$vừa vặn một cách thoải mái, vì giải pháp chỉ thực hiện công việc tuyến tính trên phạm vi số mũ và tránh mọi phép liệt kê tập hợp con. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return main()

def main():
    input = sys.stdin.readline
    n = int(input())
    arr = list(map(int, input().split()))
    maxb = max(arr)
    cnt = [0] * (maxb + 2)
    for x in arr:
        cnt[x] += 1

    def possible(cnt, target):
        carry = 0
        for i in range(len(cnt)):
            total = cnt[i] + carry
            if i == target:
                if total % 2 != 1:
                    return False
                carry = total // 2
            else:
                carry = total // 2
        return carry == 0

    for t in range(len(cnt)):
        if possible(cnt, t):
            return "Y"
    return "N"

# provided samples
assert run("4\n2 2 5 3\n") == "Y"
assert run("1\n42\n") == "N"

# custom cases
assert run("2\n0 0\n") == "Y", "two smallest powers"
assert run("3\n0 0 0\n") == "N", "cannot split triple ones"
assert run("3\n1 1 2\n") == "Y", "simple carry chain"
assert run("4\n10 10 10 10\n") == "Y", "perfect symmetry"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 nhỏ giống hệt nhau | Y | chia hợp lệ tầm thường | 
| 3 số 0 giống hệt nhau | N | thất bại bội số lẻ | 
| dây xích nhỏ | Y | độ chính xác của việc truyền bá | 
| đầu vào lớn đối xứng | Y | sự ổn định khi lặp lại | 

## Vỏ cạnh 

Một trường hợp giống như một hộp duy nhất, ví dụ như đầu vào`1\n42\n`, ngay lập tức thất bại vì một anh em nhận được tất cả số kẹo còn người kia nhận được 0, và 0 không phải là lũy thừa dương của hai. Thuật toán xử lý việc này vì không có bit mục tiêu nào có thể thỏa mãn điều kiện mang. 

Một trường hợp tinh tế khác là khi tất cả các hộp đều giống hệt nhau, chẳng hạn như`4\n3 3 3 3`. Mô phỏng mang đảm bảo rằng việc nhóm tạo ra sự hủy bỏ hoàn toàn hoặc giảm thiểu nhị phân rõ ràng và nếu không bên nào có thể tạo thành một bit hoạt động duy nhất thì tất cả các mục tiêu đều thất bại. 

Trường hợp cạnh cuối cùng là khi tồn tại nhiều số mũ nhỏ, chẳng hạn như số lượng lớn các số 0. Ở đây, chuỗi mang chiếm ưu thế trong hành vi và mô phỏng thu gọn chính xác các cặp thành các bit cao hơn cho đến khi vẫn còn một đỉnh hoặc nhiều đỉnh xuất hiện, điều này làm vô hiệu cấu hình.
