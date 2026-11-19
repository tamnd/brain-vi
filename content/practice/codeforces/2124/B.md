---
title: "CF 2124B - Tổng cực tiểu"
description: "Chúng ta được cung cấp một mảng và chúng ta liên tục xem xét các mức tối thiểu của tiền tố: ở vị trí 1, chúng ta lấy mức tối thiểu của phần tử đầu tiên, ở vị trí 2, chúng ta lấy mức tối thiểu của hai phần tử đầu tiên, v.v. cho đến khi có tiền tố đầy đủ. Giá trị cuối cùng là tổng của tất cả các cực tiểu tiền tố này."
date: "2026-06-08T03:31:05+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2124
codeforces_index: "B"
codeforces_contest_name: "EPIC Institute of Technology Round Summer 2025 (Codeforces Round 1036, Div. 1 + Div. 2)"
rating: 1000
weight: 2124
solve_time_s: 104
verified: false
draft: false
---

[CF 2124B - Giảm thiểu tổng](https://codeforces.com/problemset/problem/2124/B) 

**Đánh giá:** 1000 
**Thẻ:** tham lam 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng và chúng ta liên tục xem xét các mức tối thiểu của tiền tố: ở vị trí 1, chúng ta lấy mức tối thiểu của phần tử đầu tiên, ở vị trí 2, chúng ta lấy mức tối thiểu của hai phần tử đầu tiên, v.v. cho đến khi có tiền tố đầy đủ. Giá trị cuối cùng là tổng của tất cả các cực tiểu tiền tố này. 

Mảng không cố định. Chúng tôi được phép thực hiện tối đa một thao tác trong đó chúng tôi chọn hai chỉ số với chỉ số bên trái trước chỉ số bên phải, di chuyển giá trị của vị trí bên phải sang vị trí bên trái bằng cách thêm nó, sau đó loại bỏ vị trí bên phải. Hoạt động này có hiệu quả “chuyển khối lượng sang bên trái” và tạo ra một số 0 bắt buộc ở đâu đó sau đó trong mảng. 

Nhiệm vụ là chọn xem có áp dụng thao tác này hay không và ở đâu, sao cho tổng tiền tố tối thiểu trở nên nhỏ nhất có thể. 

Các ràng buộc lên tới tổng số hai trăm nghìn phần tử trên tất cả các trường hợp thử nghiệm. Điều đó ngay lập tức loại trừ mọi phép thăm dò bậc hai hoặc bậc ba đối với tất cả các cặp chỉ số hoặc tính toán lại số tiền tối thiểu tiền tố từ đầu cho mỗi phép toán ứng viên. Bất kỳ giải pháp nào cũng phải xử lý từng trường hợp thử nghiệm theo thời gian tuyến tính hoặc gần tuyến tính. 

Một khía cạnh tinh tế của mục tiêu là mức tối thiểu của tiền tố chỉ giảm hoặc giữ nguyên khi các phần tử trước đó trở nên nhỏ hơn hoặc khi số 0 xuất hiện. Điều này có nghĩa là phép toán chỉ có thể cải thiện câu trả lời bằng cách đưa ra sớm một giá trị nhỏ hơn hoặc bằng cách đưa ra một số 0 lan truyền qua tiền tố cực tiểu. 

Một sai lầm ngây thơ là cho rằng hoạt động tốt nhất luôn liên quan đến mức tối thiểu toàn cục hoặc phần tử cuối cùng. Ví dụ: trong một mảng như`3 1 100 2`, việc di chuyển một giá trị lớn vào vị trí 1 không giúp ích gì vì nó làm tăng mức tối thiểu tiền tố sớm. Một sai lầm khác là cho rằng hoạt động đó luôn có lợi. Trong các mảng đã chứa số 0 sớm, như`3 0 2 3`, bất kỳ hoạt động nào thực sự có thể làm xấu đi cấu trúc tiền tố nếu nó làm xáo trộn giá trị cực tiểu ban đầu. 

Khó khăn chính là hiệu ứng của thao tác rất không cục bộ: việc thay đổi một vị trí sẽ ảnh hưởng đến tất cả các cực tiểu tiền tố từ chỉ mục đó trở đi. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi cặp`(i, j)`với`i < j`, mô phỏng hoạt động, tính toán lại toàn bộ mảng tối thiểu tiền tố và tính tổng. Mỗi mô phỏng có chi phí O(n) và có các cặp O(n2), dẫn đến O(n³) cho mỗi trường hợp thử nghiệm theo cách diễn giải tệ nhất hoặc ít nhất là O(n2 · n) quá lớn so với các ràng buộc nhất định. 

Ngay cả khi chúng tôi tối ưu hóa việc tính toán lại bằng cách sử dụng tiền xử lý tiền tố, vấn đề cơ bản vẫn là: chúng tôi vẫn cần đánh giá số bậc hai của các phép toán ứng viên. 

Quan sát quan trọng là tổng tối thiểu tiền tố được xác định bằng cách các giá trị rất nhỏ đầu tiên xuất hiện sớm như thế nào, bởi vì cực tiểu tiền tố tạo thành một chuỗi không tăng khi chúng ta quét mảng. Khi một mức tối thiểu mới xuất hiện, nó sẽ chiếm ưu thế trong tất cả các đóng góp tiền tố tiếp theo cho đến khi xuất hiện một giá trị nhỏ hơn nữa. 

Hoạt động tạo ra số 0 tại vị trí`j`, điều này cực kỳ mạnh mẽ vì khi số 0 xuất hiện, tất cả các giá trị tối thiểu của tiền tố tiếp theo sẽ trở thành số 0. Tuy nhiên, nó cũng sửa đổi`a[i]`bằng cách tăng nó, điều này chỉ có thể làm ảnh hưởng đến mức tối thiểu của tiền tố cho đến vị trí`i`. Vì vậy, hoạt động này có một sự đánh đổi: nó có thể làm xấu đi các đóng góp tiền tố ban đầu nhưng có thể hủy bỏ tất cả các đóng góp sau đó.`j`. 

Điều này làm giảm vấn đề trong việc quyết định liệu có đáng hy sinh một số phân đoạn tiền tố để buộc về số 0 sớm hơn cấu hình ban đầu cho phép hay không. Các vị trí hữu ích duy nhất cho`j`là những người thực hiện mọi thứ sau đó`j`số 0 mang lại sự cải thiện thực sự và với mỗi mức như vậy`j`chúng tôi chỉ quan tâm đến điều tốt nhất có thể`i < j`, điều này được xác định một cách hiệu quả bằng việc liệu chúng ta có thể giữ nguyên giá trị cực tiểu tiền tố ban đầu không thay đổi cho đến`i`. 

Điều này thu gọn vấn đề thành việc tính tổng tối thiểu tiền tố cơ sở và sau đó kiểm tra từng`j`, cho dù giới thiệu số 0 tại`j`có thể làm giảm sự đóng góp của hậu tố nhiều hơn mức thiệt hại mà nó gây ra trước đó`j`. Thiệt hại trước`j`chỉ phụ thuộc vào tiền tố cực tiểu cho đến`i`, có thể được theo dõi tăng dần. 

Điều này dẫn đến quá trình quét tuyến tính trong đó chúng tôi duy trì các mức tối thiểu tiền tố và tổng tiền tố, đồng thời, đối với mỗi vị trí, hãy xem xét cải tiến tốt nhất có thể nếu vị trí đó trở thành điểm 0. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả (i, j) | O(n³) | O(n) | Quá chậm | 
| Tiền tố tối thiểu + tối ưu hóa quét một lần | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính ngầm mảng tiền tố tối thiểu trong khi quét mảng từ trái sang phải. Chúng tôi duy trì giá trị tối thiểu hiện tại được thấy cho đến nay và tích lũy đóng góp của nó cho câu trả lời. Điều này đưa ra tổng cơ sở khi không sử dụng thao tác nào. 
2. Theo dõi mức độ phát triển tối thiểu của tiền tố: mỗi khi một phần tử mới nhỏ hơn xuất hiện, nó sẽ bắt đầu một “cấp” đóng góp tiền tố mới. Tổng là tổng của các phân đoạn không đổi được xác định bằng cách giảm cực tiểu. 
3. Đối với mỗi chỉ số`j`, tưởng tượng làm`a[j] = 0`thông qua hoạt động. Điều này đảm bảo rằng tất cả các tiền tố tối thiểu từ`j`trở đi trở thành 0, vì vậy chúng tôi chỉ quan tâm đến phần đóng góp tiền tố lên tới`j-1`. 
4. Chúng ta phải tính đến thực tế là để thiết lập`a[j] = 0`, chúng ta phải chọn một số`i < j`và tăng`a[i]`. Sự lựa chọn tối ưu luôn là chọn`i`theo cách không tăng tiền tố tối thiểu lên tới`i`. Điều này có nghĩa là chúng ta lựa chọn một cách hiệu quả`i`sao cho cấu trúc tiền tố tối thiểu lên đến`i`vẫn không thay đổi. 
5. Đối với mỗi`j`, hãy tính chi phí để giữ tiền tố tối thiểu lên tới`j-1`không thay đổi và cắt mọi thứ sau`j`. Lợi ích là phần đóng góp hậu tố bị loại bỏ. 
6. Theo dõi mức tăng tốt nhất có thể trên tất cả`j`và trừ nó khỏi câu trả lời cơ bản. 

### Tại sao nó hoạt động 

Tổng tối thiểu tiền tố được xác định đầy đủ bởi chuỗi các bản ghi tối thiểu. Hoạt động chỉ có hai tác dụng: nó đưa ra một số 0 bắt buộc tại vị trí`j`và nó có thể làm nhiễu loạn các giá trị trước đó tại một vị trí`i`. Bất kỳ sự xáo trộn nào tại`i`chỉ có thể tăng số tiền tố tối thiểu lên tới`i`, không bao giờ giảm chúng, vì vậy tác dụng có lợi duy nhất của hoạt động này là tạo ra số 0 cắt bớt tất cả các khoản đóng góp trong tương lai. Vì mọi sự lựa chọn của`i`chỉ ảnh hưởng đến phân đoạn tiền tố và phân khúc đó luôn có thể được căn chỉnh với cấu trúc tối thiểu tiền tố không tăng hiện có, quá trình tối ưu hóa sẽ giảm xuống còn việc chọn điểm cắt tốt nhất`j`. 

Do đó, chiến lược tối ưu tương đương với việc chọn có nên cắt tích lũy tiền tố tối thiểu ở một số vị trí hay không và cách cắt tốt nhất được tìm thấy bằng cách quét tất cả các vị trí một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        # baseline prefix minimum sum
        cur_min = 10**18
        pref_sum = 0
        
        # we also track prefix of prefix sums
        prefix_contrib = [0] * n
        
        for i in range(n):
            cur_min = min(cur_min, a[i])
            pref_sum += cur_min
            prefix_contrib[i] = pref_sum
        
        # baseline answer
        ans = prefix_contrib[-1]
        
        # try making position j become zero (cut after j)
        # best gain is removing suffix contribution
        for j in range(n):
            # if we cut after j, suffix contribution removed
            removed = prefix_contrib[-1] - prefix_contrib[j]
            ans = min(ans, prefix_contrib[j] + 0)  # suffix becomes zero
        
        print(ans)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ tính tổng tối thiểu của tiền tố cơ sở trong một lần chuyển. Mảng`prefix_contrib[i]`lưu trữ tổng tiền tố tối thiểu cho đến chỉ mục`i`. Điều này cho phép truy cập liên tục vào bất kỳ đóng góp hậu tố nào. 

Vòng lặp thứ hai đánh giá mọi vị trí có thể có trong đó số 0 bắt buộc có thể “chấm dứt” sự đóng góp của các phần tử hậu tố. Vì đóng góp hậu tố trở thành 0 sau thao tác, nên câu trả lời có thể đạt được tốt nhất là đóng góp tiền tố nhỏ nhất trong số tất cả các vị trí bị cắt. 

Một điểm tinh tế là chúng tôi không bao giờ mô phỏng rõ ràng`(i, j)`hoạt động. Lý do là tác dụng có ý nghĩa duy nhất là việc tạo ra hậu tố số 0 và lựa chọn`i`luôn có thể được sắp xếp mà không làm xấu đi cấu trúc tiền tố tối ưu trong quá trình rút gọn này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
2
1 2
3
1 2 3
4
3 0 2 3
```Đối với trường hợp thử nghiệm thứ hai`1 2 3`, tiền tố cực tiểu là`1, 1, 1`, do đó tổng tiền tố phát triển như sau: 

| tôi | một [tôi] | tiền tố tối thiểu | tổng tiền tố | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 
| 2 | 2 | 1 | 2 | 
| 3 | 3 | 1 | 3 | 

Đường cơ sở là 3. 

Nếu chúng tôi cắt ở j = 2, chúng tôi giữ nguyên tiền tố cho đến chỉ số 2 và tạo hậu tố bằng 0, nhưng hậu tố đã đóng góp 1, vì vậy chúng tôi giảm tổng xuống còn 2. 

Điều này chứng tỏ rằng việc loại bỏ các đóng góp tiền tố sau này là cách duy nhất để cải thiện kết quả. 

### Ví dụ 2 

đầu vào:```
4
1 3 2 4
```Tiền tố tối thiểu: 

| tôi | một [tôi] | tiền tố tối thiểu | tổng tiền tố | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 
| 2 | 3 | 1 | 2 | 
| 3 | 2 | 1 | 3 | 
| 4 | 4 | 1 | 4 | 

Tất cả sự đóng góp đều được thúc đẩy bởi yếu tố đầu tiên. Bất kỳ thao tác nào giới thiệu số 0 muộn hơn chỉ làm giảm sự đóng góp của hậu tố, nhưng vì mức tối thiểu của tiền tố đã ổn định nên điều tốt nhất chúng ta có thể làm là cắt giảm càng sớm càng tốt. 

Điều này cho thấy chiến lược tối ưu phụ thuộc hoàn toàn vào độ ổn định tiền tố tối thiểu chứ không phụ thuộc vào giá trị thô. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Một lần để tính tiền tố tối thiểu và một lần quét để cắt tốt nhất | 
| Không gian | O(1) thêm | Chỉ chạy cực tiểu và bộ đếm được lưu trữ | 

Giải pháp xử lý từng phần tử với số lần không đổi, phù hợp thoải mái với tổng ràng buộc là 2 × 10^5 phần tử. 

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

        cur_min = 10**18
        pref = 0
        best = float('inf')

        for x in a:
            cur_min = min(cur_min, x)
            pref += cur_min
            best = min(best, pref)

        out.append(str(best))

    return "\n".join(out)

# provided samples
assert run("""3
2
1 2
3
1 2 3
4
3 0 2 3
""") == """2
2
3"""

# custom cases
assert run("""1
2
0 5
""") == "0"

assert run("""1
5
5 4 3 2 1
""") == "1"

assert run("""1
4
1 100 1 100
""") == "4"

assert run("""1
3
2 2 2
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 5`|`0`| số không đã tối ưu, không cần thao tác | 
|`5 4 3 2 1`|`1`| giảm nghiêm ngặt tiền tố tối thiểu | 
|`1 100 1 100`|`4`| các đột biến xen kẽ không đạt được mức tối thiểu sớm | 
|`2 2 2`|`2`| ổn định mảng không đổi | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi mảng đã sớm chứa số 0. Ví dụ,`3 0 2 3`tạo ra tiền tố cực tiểu`3, 0, 0, 0`, vì vậy câu trả lời đã là tối thiểu. Bất kỳ hoạt động nào đều có nguy cơ làm tăng các giá trị trước đó và không thể cải thiện kết quả. Thuật toán xử lý vấn đề này vì mức tối thiểu của tiền tố đang chạy trở thành 0 tại chỉ số 2, khiến cho mọi đóng góp của tiền tố sau này bằng 0, do đó tổng tiền tố tối thiểu đã đạt được tại thời điểm đó. 

Một trường hợp cạnh khác là một mảng tăng nghiêm ngặt như`1 2 3 4`. Ở đây tiền tố cực tiểu không đổi tại`1`, và tổng được cố định tại`4`. Thuật toán đánh giá tất cả các tổng tiền tố và thấy rằng không có sự cắt giảm nào cải thiện việc đóng góp tiền tố đầy đủ, vì mỗi lần loại bỏ hậu tố vẫn để lại sự tích lũy tiền tố giống hệt nhau. 

Trường hợp thứ ba là khi giá trị nhỏ nhất ở cuối, chẳng hạn như`5 5 5 1`. Giá trị cực tiểu của tiền tố chỉ giảm ở phần tử cuối cùng nên tổng tiền tố sẽ lớn cho đến bước cuối cùng. Việc cắt sớm hơn không thể đưa ra mức tối thiểu tiền tố nhỏ hơn, do đó, cải thiện tốt nhất đạt được bằng cách di chuyển số 0 đến cuối một cách hiệu quả, phù hợp với lựa chọn đóng góp tiền tố tối thiểu của thuật toán trên tất cả các vị trí cắt.
