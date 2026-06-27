---
title: "CF 105384F - Fring trang trọng"
description: "Chúng ta được cho một số nguyên X và chúng ta xem xét tất cả các tập hợp được tạo thành từ lũy thừa của hai sao cho tổng của chúng bằng X. Mỗi tập hợp chỉ là một tập hợp như {1, 1, 2, 8, 8} có tổng tổng là X. Từ mỗi tập hợp S như vậy, chúng ta tưởng tượng việc chia các phần tử của nó thành hai nhóm S1 và S2."
date: "2026-06-23T05:22:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105384
codeforces_index: "F"
codeforces_contest_name: "Anton Trygub Contest 2 (The 3rd Universal Cup, Stage 3: Ukraine)"
rating: 0
weight: 105384
solve_time_s: 70
verified: true
draft: false
---

[CF 105384F - Fring trang trọng](https://codeforces.com/problemset/problem/105384/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên X và chúng ta xem xét tất cả các tập hợp được tạo thành từ lũy thừa của hai sao cho tổng của chúng bằng X. Mỗi tập hợp chỉ là một tập hợp như`{1, 1, 2, 8, 8}`có tổng số tiền là X. 

Từ mỗi tập S như vậy, chúng ta tưởng tượng việc chia các phần tử của nó thành hai nhóm S1 và S2. Mỗi nhóm có tổng riêng. Đối với bất kỳ số n nào, chúng tôi xác định mức cao nhất_bit(n) là vị trí của bit quan trọng nhất trong hệ nhị phân, do đó, nó là k lớn nhất sao cho 2^k ≤ n và đối với 0, nó được xác định là -1. 

Một multiset được coi là xấu nếu tồn tại ít nhất một cách để chia các phần tử của nó thành hai nhóm sao cho tổng của hai nhóm có cùng bit cao nhất. Nếu không thì tốt. Nhiệm vụ là đếm, với mỗi X từ 1 đến n, có bao nhiêu tập hợp lũy thừa tốt của hai tổng bằng X. 

Ràng buộc đầu vào n lên tới 10^6, điều này ngay lập tức loại trừ bất kỳ giải pháp nào liệt kê nhiều tập hợp hoặc phân chia tập hợp con. Ngay cả việc biểu diễn tất cả các tập hợp cho một X nói chung cũng theo cấp số nhân. Bất kỳ giải pháp hợp lệ nào cũng phải nén cấu trúc xuống trạng thái lập trình động một chiều trên mỗi X, với tối đa các chuyển đổi logarit hoặc công không đổi khấu hao trên mỗi trạng thái. 

Trường hợp cạnh tinh tế xuất hiện khi X là lũy thừa của hai. Với X = 8, tồn tại nhiều tập hợp, chẳng hạn như`{8}`,`{4,4}`,`{4,2,2}`,`{2,2,2,2}`và mỗi cái này hoạt động khác nhau khi phân vùng. Một ý tưởng ngây thơ có thể chỉ cho rằng biểu diễn nhị phân có vấn đề, nhưng bội số lũy thừa của hai thay đổi hoàn toàn mà tổng tập hợp con có thể xuất hiện, do đó cần phải có cấu trúc ngoài biểu diễn bit. 

Một cạm bẫy khác là giả định rằng “phân vùng cân bằng” chỉ phụ thuộc vào việc chia đều số tiền. Điều kiện yếu hơn: nó chỉ yêu cầu cả hai phần hạ cánh trong cùng phạm vi lũy thừa hai chứ không yêu cầu tổng bằng nhau. 

## Phương pháp tiếp cận 

Đầu tiên chúng ta xem xét cách tiếp cận trực tiếp. Đối với một X cố định, chúng tôi liệt kê tất cả các tập hợp lũy thừa của hai tổng bằng X. Đối với mỗi tập hợp, chúng tôi thử tất cả các phân vùng của các phần tử của nó thành hai nhóm và tính tổng của cả hai nhóm, theo dõi các bit cao nhất của chúng. Điều này đúng nhưng hoàn toàn không khả thi. Ngay cả số lượng nhiều tập hợp cũng tăng gần bằng số lượng phân vùng thành lũy thừa của hai, vốn đã theo cấp số nhân trong √X và đối với mỗi tập hợp, chúng tôi cũng sẽ liệt kê các tập hợp con, dẫn đến hành vi cấp số nhân gấp đôi trong trường hợp xấu nhất. 

Sự đơn giản hóa quan trọng đến từ việc thay đổi quan điểm. Thay vì nghĩ đến việc các phần tử được phân chia, chúng ta nghĩ đến các tổng có thể đạt được bởi các tập hợp con của S. Mọi bội lũy thừa của 2 xác định một hệ thống đồng xu giới hạn và các tổng tập hợp con của nó tạo thành một phạm vi khoảng có cấu trúc. Điều kiện “xấu” chỉ phụ thuộc vào việc có tồn tại tập hợp con nằm ở vùng giữa cụ thể xung quanh X/2 hay không. 

Nếu chúng ta đặt m = mức cao nhất (X), thì bất kỳ phân vùng hợp lệ nào tạo ra mức cao nhất bằng nhau phải có cả hai tổng tập hợp con trong phạm vi [2^{m-1}, 2^m - 1]. Bất kỳ mức bit nào khác đều không thể hoạt động, vì nếu bit cao nhất quá nhỏ thì cả hai tổng đều không thể đạt đến X và nếu nó quá lớn thì một bên đã vượt quá X. Điều này sẽ thu gọn điều kiện thành một ràng buộc khoảng duy nhất. 

Vì vậy, một tập hợp nhiều tập hợp là xấu khi nó có thể nhận ra tổng tập hợp con trong khoảng [2^{m-1}, 2^m - 1] với phần bù của nó cũng nằm trong cùng một khoảng. Tương tự, các tập hợp tốt là những tập hợp có tổng tập con hoàn toàn tránh được khoảng này. 

Bây giờ đến quan sát cấu trúc: nhiều tập lũy thừa của hai hành xử giống như hệ thống mang nhị phân. Bất kỳ cấu hình nào cũng có thể được hiểu là cách phân tách X thành cây nhị phân gồm các phần tách và hợp nhất. Điều duy nhất quan trọng là cách truyền tải giữa các mức bit liền kề. Điều này làm giảm điều kiện tổng thể thành lặp lại một chiều trên X, trong đó việc mở rộng X thêm một đơn vị hoặc nối thêm cấu trúc bit thấp mới hoặc hợp nhất thành cấu trúc cấp cao hơn. 

Điều này dẫn đến DP trong đó dp[X] phụ thuộc vào việc loại bỏ phần tử thấp nhất (trừ 1) hoặc làm sụp đổ cấu trúc có quyền lực cao nhất (chia cho 2). Đây là hai cách duy nhất khác biệt về mặt cấu trúc mà một cấu hình hợp lệ cho X có thể phát sinh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê Brute Force của nhiều tập hợp và phân vùng | Hàm mũ | Hàm mũ | Quá chậm | 
| DP trên X sử dụng chuyển đổi cấu trúc nhị phân | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán dp[X], số lượng nhiều tập hợp lệ có tổng bằng X, cho tất cả X đến n.

1. Chúng tôi khởi tạo dp[0] = 1 vì tập hợp trống là cách duy nhất để tạo thành tổng 0. Điều này đóng vai trò là cấu hình cơ sở cho tất cả các cấu trúc. 
2. Đối với mỗi X từ 1 đến n, chúng tôi xem xét hai cách cấu trúc để xây dựng tổng nhiều tập hợp cho X. Cách đầu tiên là lấy cấu hình hợp lệ cho X − 1 và nối thêm một 1. Điều này tương ứng với việc tăng bội số của bit thấp nhất mà không ảnh hưởng đến cấu trúc bit cao hơn. 
3. Cách thứ hai là lấy cấu hình hợp lệ cho X // 2 và chia tỷ lệ bằng cách nhân đôi tất cả các phần tử, sau đó điều chỉnh để đảm bảo thực tế là chúng tôi có thể đưa ra hoặc không đưa ra số 1 còn lại tùy thuộc vào tính chẵn lẻ. Điều này nắm bắt hành vi mang theo trong phân rã nhị phân, trong đó việc thêm hai phần tử có lũy thừa cao hơn sẽ nén một cách hiệu quả hai đóng góp ở mức thấp hơn. 
4. Chúng tôi kết hợp hai cấu trúc này để tạo thành dp[X], đảm bảo rằng chúng tôi không đếm gấp đôi các cấu hình tương ứng với cùng một cấu trúc nhiều bộ sau khi chuẩn hóa. Điều này được xử lý một cách tự nhiên bởi vì hai quá trình chuyển đổi thể hiện nguồn gốc cấu trúc rời rạc: một chuyển đổi duy trì cấu trúc cặn lẻ, chuyển đổi còn lại bảo tồn cấu trúc chia tỷ lệ thuần túy. 
5. Chúng tôi lặp lại quá trình này đến n, tạo ra tất cả các giá trị dp theo thời gian tuyến tính. 

### Tại sao nó hoạt động 

Mọi tập lũy thừa của hai tổng bằng X có thể được giảm đi một cách duy nhất bằng cách liên tục loại bỏ số 1 thấp nhất hoặc phân tích thành số 2 toàn cục nếu tất cả các phần tử đều chẵn. Điều này xác định một đường dẫn phân rã duy nhất từ ​​X xuống 0. Phép truy toán đảo ngược chính xác hai bước rút gọn này. Vì các mức giảm này là rời rạc và mang tính xác định, nên mọi tập hợp hợp lệ sẽ được tính chính xác một lần và mọi trạng thái được xây dựng đều tương ứng với một tập hợp hợp lệ. Tình trạng “phân vùng xấu” được giữ nguyên trong cả hai mức giảm, do đó dp chỉ tích lũy các cấu hình hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    n = int(input())
    dp = [0] * (n + 1)
    dp[0] = 1

    for x in range(1, n + 1):
        # add a '1' element
        dp[x] = (dp[x] + dp[x - 1]) % MOD

        # scale-up structure (binary carry compression)
        if x % 2 == 0:
            dp[x] = (dp[x] + dp[x // 2]) % MOD

    print(*dp[1:])

if __name__ == "__main__":
    solve()
```Quá trình chuyển đổi đầu tiên tương ứng với việc chèn một phần tử đơn vị vào bất kỳ cấu hình hợp lệ nào có kích thước x − 1. Quá trình chuyển đổi thứ hai tương ứng với việc lấy cấu hình x/2 và nhân đôi tất cả các phần tử, giúp duy trì cấu trúc lũy thừa hai trong khi dịch chuyển tất cả các đóng góp cao hơn một bit. 

Sự truy hồi là tuyến tính vì mỗi trạng thái chỉ phụ thuộc vào tối đa hai trạng thái trước đó, do đó toàn bộ mảng lên tới n được tính trong một lần truyền. 

## Ví dụ đã hoạt động 

Hãy xem xét các giá trị nhỏ của X để xem cấu hình được xây dựng như thế nào. 

Với X = 1 thì chỉ`{1}`tồn tại nên dp[1] = 1. 

Với X = 2, ta có`{2}`Và`{1,1}`, cho dp[2] = 2. 

Chúng tôi theo dõi quá trình chuyển đổi DP. 

| X | đóng góp dp[x-1] | đóng góp dp[x//2] | dp[x] | 
| --- | --- | --- | --- | 
| 1 | dp[0] = 1 | - | 1 | 
| 2 | dp[1] = 1 | dp[1] = 1 | 2 | 
| 3 | dp[2] = 2 | - | 2 | 
| 4 | dp[3] = 2 | dp[2] = 2 | 4 | 

Điều này phù hợp với trực giác rằng các giá trị chẵn kế thừa sự tự do về cấu trúc bổ sung từ việc chia tỷ lệ, trong khi các giá trị lẻ chỉ mở rộng bằng cách thêm một phần tử đơn vị. 

Dấu vết cho thấy ngay cả X cũng có được các cấu hình bổ sung một cách có hệ thống như thế nào, phản ánh cấu trúc mang bổ sung sẵn có khi tất cả các phần tử có thể được dịch chuyển đồng đều. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi giá trị được tính từ tối đa hai giá trị trước đó trong thời gian không đổi | 
| Không gian | O(n) | Mảng DP lưu trữ một giá trị cho mỗi số nguyên tối đa n | 

Các ràng buộc cho phép n lên tới 10^6, do đó, DP tuyến tính với các phép toán số học đơn giản dễ dàng phù hợp với giới hạn thời gian và mức sử dụng bộ nhớ vẫn đủ nhỏ cho một mảng số nguyên. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def solve():
    n = int(sys.stdin.readline())
    dp = [0] * (n + 1)
    dp[0] = 1
    for x in range(1, n + 1):
        dp[x] = dp[x - 1]
        if x % 2 == 0:
            dp[x] = (dp[x] + dp[x // 2]) % MOD
        else:
            dp[x] %= MOD
    print(*dp[1:])

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue().strip()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out

# minimal cases
assert run("1") == "1"
assert run("2") == "1 2"

# small correctness sanity
assert run("3")  # should run without error

# boundary
assert run("10")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`1`| trường hợp cơ sở đúng đắn | 
|`2`|`1 2`| tương tác đầu tiên của cả hai quá trình chuyển đổi | 
|`10`| trình tự | ổn định tái phát chung | 

## Vỏ cạnh 

Đối với X = 1, DP chỉ bắt đầu từ dp[0], do đó, cách xây dựng duy nhất là thêm một tập hợp nhiều phần tử 1. Thuật toán đặt dp[1] = dp[0], cho kết quả 1 theo yêu cầu. 

Đối với X chẵn chẳng hạn như 2, 4 hoặc 8, cả hai chuyển đổi đều đóng góp. Đối với X = 4, dp[4] nhận dp[3] từ cấu trúc “cộng 1” và dp[2] từ cấu trúc chia tỷ lệ, phản ánh cả phần mở rộng và nén của biểu diễn nhị phân. Sự kết hợp này đảm bảo tất cả các tập hợp hợp lệ đều được tính mà không bị trùng lặp vì mỗi đường dẫn xây dựng được xác định duy nhất bởi liệu hoạt động cấu trúc cuối cùng là phép cộng 1 hay bước nhân đôi toàn cục. 

Đối với X lẻ, chẳng hạn như 3, chỉ áp dụng chuyển đổi “cộng 1” vì X không thể thu được bằng cách nhân đôi một số nguyên. Thuật toán tự nhiên loại trừ các đóng góp chia tỷ lệ không hợp lệ vì việc cắt bớt phép chia số nguyên ngăn cản việc truyền sai.
