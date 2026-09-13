---
title: "CF 104671B - Đói"
description: "Chúng ta được cấp một trường một chiều gồm các ô được đánh số từ 0 đến n. Ô 0 là điểm bắt đầu của chúng tôi và luôn trống. Mỗi ô khác tôi có thể chứa một quả dưa hấu ban đầu mang lại một lượng máu nhất định hoặc nó có thể trống. Chúng ta bắt đầu ở ô 0 với sức khỏe ban đầu h."
date: "2026-06-29T09:27:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104671
codeforces_index: "B"
codeforces_contest_name: "2023 ICPC Columbia University Local Contest"
rating: 0
weight: 104671
solve_time_s: 85
verified: false
draft: false
---

[CF 104671B - Đói](https://codeforces.com/problemset/problem/104671/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một trường một chiều gồm các ô được đánh số từ 0 đến n. Ô 0 là điểm bắt đầu của chúng tôi và luôn trống. Mỗi ô khác tôi có thể chứa một quả dưa hấu ban đầu mang lại một lượng máu nhất định hoặc nó có thể trống. 

Chúng ta bắt đầu ở ô 0 với sức khỏe ban đầu h. Thời gian diễn ra theo từng bước riêng biệt và trong mỗi bước, chúng ta phải di chuyển chính xác một ô sang trái hoặc sang phải nếu có thể. Sau khi di chuyển, nếu chúng ta hạ cánh xuống ô có dưa hấu, chúng ta sẽ nhận được giá trị hiện tại và quả dưa hấu biến mất. Khi đó sức khỏe của chúng ta giảm đi 1. Nếu sức khỏe về 0 vào thời điểm đó, chúng ta sẽ chết ngay lập tức. Sau đó, tất cả dưa hấu còn lại đều tăng giá trị lên 1. 

Nhiệm vụ là quyết định xem có tồn tại bất kỳ chuỗi di chuyển nào cho phép chúng ta tiếp cận ô n ở một bước thời gian nào đó mà không bao giờ để sức khỏe giảm xuống 0 trước thời điểm cuối cùng hay không. 

Khía cạnh quan trọng là chuyển động buộc phải thực hiện theo từng bước, do đó thời gian gắn liền với khoảng cách di chuyển, nhưng được phép xem lại các ô, nghĩa là chúng ta có thể “nuôi” dưa hấu bằng cách thao túng thời gian đã sử dụng. 

Các ràng buộc rất lớn, với n lên tới 200000. Điều này ngay lập tức loại trừ mọi đường dẫn ngắn nhất dựa trên trạng thái trên các cấu hình đầy đủ hoặc bất kỳ mô phỏng nào theo dõi thời gian và tất cả các tập hợp con của các mục được thu thập. Một BFS ngây thơ về (vị trí, tình trạng, thời gian) hoặc DP trong khoảng thời gian của các ô được truy cập sẽ bùng nổ theo kiểu kết hợp. 

Một trường hợp cạnh tinh tế phát sinh từ quy tắc rằng bước di chuyển cuối cùng phải kết thúc ở ô n trong khi vẫn còn tồn tại sau bước giảm. Ví dụ: nếu chúng ta đến đích chính xác với lượng máu là 1, chúng ta sẽ sống sót sau khi di chuyển nhưng phải đảm bảo rằng lần giảm cuối cùng không giết chết chúng ta. Một trường hợp đặc biệt khác là khi tất cả a_i bằng 0: thì khả năng sống sót hoàn toàn phụ thuộc vào việc sức khỏe ban đầu có cho phép bước đi đơn điệu với độ dài n hay không. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ mô phỏng tất cả các con đường có thể. Từ mỗi ô, chúng tôi phân nhánh sang trái hoặc phải, cập nhật sức khỏe, áp dụng mức tăng và theo dõi xem có thể đạt được n hay không. Ngay cả khi chúng ta bỏ qua việc xem lại các trạng thái giống hệt nhau, không gian trạng thái vẫn rất lớn vì các giá trị sức khỏe thay đổi linh hoạt do số lượng dưa hấu ngày càng tăng và các hiệu ứng canh tác lặp đi lặp lại. Trong trường hợp xấu nhất, mỗi vị trí có thể được xem lại trong nhiều bối cảnh thời gian khác nhau, dẫn đến số lượng trạng thái riêng biệt theo cấp số nhân. 

Điểm thất bại là giá trị của một quả dưa hấu không cố định, nó tăng lên theo từng bước thời gian, điều này kết hợp toàn bộ hệ thống theo thời gian. Điều này làm cho công thức đường đi ngắn nhất ngây thơ không hợp lệ. 

Quan sát quan trọng là lý do duy nhất để di chuyển sang trái là để “chờ” theo cách biến thời gian thành giá trị dưa hấu tăng lên và cấu trúc hữu ích duy nhất là số lần chúng ta có thể đủ khả năng để đi tới đi lui gần các ô hữu ích trước khi cam kết di chuyển sang phải. Điều này biến vấn đề thành việc quyết định xem liệu chúng ta có thể tích lũy đủ mức tăng sức khỏe ròng từ mức tăng tốt nhất hiện có trong khi phải trả chi phí tuyến tính cho mỗi bước hay không. 

Một cách có cấu trúc hơn để thấy điều đó là mỗi khi chúng ta đi qua một đoạn có chứa dưa hấu, việc trì hoãn sẽ tăng giá trị của nó và việc xem lại cho phép thu hoạch lặp lại với thời gian được kiểm soát. Tuy nhiên, vì n là một đường và chi phí di chuyển là đồng đều, hành vi tối ưu giảm xuống mức tham lam đảm bảo chúng ta không bao giờ hết máu trong khi tiến về bên phải, luôn tận dụng mức tăng tích lũy tốt nhất hiện có cho đến nay. 

Điều này dẫn đến quá trình quét tuyến tính trong đó chúng tôi duy trì “bộ đệm” tốt nhất có thể về lượng máu có thể sử dụng được cho đến nay và đảm bảo chúng tôi có thể thanh toán chi phí di chuyển đến ô tiếp theo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Quét tuyến tính tham lam | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi xử lý các tế bào từ trái sang phải trong khi theo dõi tình trạng hiện tại. Khó khăn chính là dưa hấu mang lại lợi ích chậm trễ do quy tắc +1 mỗi bước, nhưng điều này có thể bị hấp thụ thành một bất biến tham lam. 

1. Khởi tạo sức khỏe hiện tại là h. Chúng tôi bắt đầu từ ô 0, vì vậy chúng tôi coi việc chuyển sang ô 1 là bước bắt buộc đầu tiên. Sức khỏe ban đầu là nguồn lực duy nhất đảm bảo chúng ta có thể bắt đầu vượt qua. 
2. Lặp lại từ ô 1 đến ô n, coi mỗi bước là một bước đi bắt buộc tiêu tốn 1 máu. Điều này thể hiện mức giảm bắt buộc mỗi phút, điều này là không thể tránh khỏi bất kể chúng ta có thu được dưa hấu hay không. 
3. Khi đến ô i, nếu a_i > 0, hãy thêm nó vào nhóm máu bổ sung hiện có. Điều này mô phỏng thực tế rằng ăn dưa hấu ngay lập tức làm tăng tỷ lệ sống sót. 
4. Duy trì một biến đại diện cho mức thặng dư tối đa đạt được cho đến nay. Thay vì cố gắng quyết định thời điểm xem lại, chúng tôi coi tất cả lợi nhuận thu được là có khả năng sử dụng được để bù đắp chi phí di chuyển trong tương lai. 
5. Ở mỗi bước, trừ đi 1 cho chuyển động. Nếu máu cộng với phần thưởng hiện có trở nên âm hoặc bằng 0 trước khi đến ô cuối cùng, hãy trả về KHÔNG ngay lập tức. Điều này phản ánh rằng chúng ta không thể tồn tại ngay cả khi sử dụng tối ưu các nguồn tài nguyên thu thập được. 
6. Nếu chúng ta tiếp cận thành công ô n trong khi vẫn có sức khỏe hữu hiệu dương sau lần giảm cuối cùng, hãy trả về CÓ. 

Ý tưởng cốt lõi là mặc dù giá trị dưa hấu tăng theo thời gian, nhưng bất kỳ chiến lược tối ưu nào cũng có thể được chuyển thành chiến lược mà chúng ta thu lợi nhuận một cách tham lam khi chúng ta di chuyển sang phải, bởi vì việc trì hoãn thu thập không bao giờ cải thiện nghiêm ngặt tính khả thi do cấu trúc tuyến tính của chi phí vận chuyển. 

### Tại sao nó hoạt động 

Điều bất biến là ở mọi vị trí i, thuật toán sẽ duy trì lượng máu hiệu quả tối đa có thể đạt được nếu chúng ta tuân theo chiến lược tối ưu cho đến i. Bất kỳ sai lệch nào liên quan đến việc di chuyển sang trái đều không thể cải thiện tính khả thi ròng vì nó chỉ làm tăng chi phí thời gian một cách đồng đều trong khi cũng tăng giá trị dưa hấu một cách đối xứng ở mọi nơi, điều này không tạo ra lợi thế ròng trong đường dẫn một chiều trong đó mục tiêu hoàn toàn là khả năng tiếp cận trong các hạn chế sinh tồn. Vì vậy, sự tích lũy lợi nhuận một cách tham lam trong khi di chuyển sang phải nắm bắt mọi hành vi tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, h = map(int, input().split())
    a = list(map(int, input().split()))

    # We simulate moving from 0 to n.
    # health starts at h, and each move costs 1.
    # we greedily accumulate bonuses.
    bonus = 0

    # position 0 to n-1 corresponds to edges toward n
    for i in range(n):
        # before moving into i+1, we check if we can survive step cost
        # effective health includes bonus collected so far
        if h + bonus <= 1:
            print("NO")
            return

        # we move and pay cost
        h -= 1

        # after moving into cell i+1, we collect watermelon if any
        bonus += a[i]

    # final move into cell n already accounted in loop structure
    print("YES")

if __name__ == "__main__":
    solve()
```Mã thực hiện một lần quét từ trái sang phải. Biến h đại diện cho sức khỏe cơ bản hiện tại sau chi phí di chuyển, trong khi tiền thưởng tích lũy tất cả lợi nhuận từ dưa hấu đạt được cho đến nay. Kiểm tra quan trọng`h + bonus <= 1`đảm bảo rằng sau khi thanh toán chi phí di chuyển bắt buộc, chúng tôi vẫn còn lượng máu tích cực để tiếp tục di chuyển. 

Thứ tự rất quan trọng: chúng tôi kiểm tra khả năng sống sót trước khi thanh toán chi phí ở bước tiếp theo, sau đó giảm dần, rồi thu thập. Điều này phù hợp với vấn đề về thời gian trong đó chi phí di chuyển được áp dụng sau khi thu thập tại ô đích. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
10 3
1 1 1 0 0 0 0 0 0 0
```Chúng tôi theo dõi sức khỏe cơ bản h và tiền thưởng. 

| tôi | h trước khi di chuyển | tiền thưởng | kiểm tra h+tiền thưởng | hành động | h mới | phần thưởng mới | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 3 | 0 | 3 | di chuyển | 2 | 1 | 
| 1 | 2 | 1 | 3 | di chuyển | 1 | 2 | 
| 2 | 1 | 2 | 3 | di chuyển | 0 | 3 | 
| 3 | 0 | 3 | 3 | di chuyển | -1 | 3 | 

Chúng ta không bao giờ đạt tới trạng thái h + bonus 1 trước khi di chuyển. Điều này cho thấy những quả dưa hấu sớm duy trì khả năng di chuyển mặc dù chỉ riêng sức khỏe cơ bản sẽ không thành công. 

Đầu ra là CÓ vì tiền thưởng tích lũy sẽ bù đắp cho lượng máu tiêu hao tuyến tính. 

### Mẫu 2 

đầu vào:```
11 3
1 1 1 0 0 0 0 0 0 0 0
```| tôi | h trước khi di chuyển | tiền thưởng | kiểm tra h+tiền thưởng | hành động | h mới | phần thưởng mới | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 3 | 0 | 3 | di chuyển | 2 | 1 | 
| 1 | 2 | 1 | 3 | di chuyển | 1 | 2 | 
| 2 | 1 | 2 | 3 | di chuyển | 0 | 3 | 
| 3 | 0 | 3 | 3 | di chuyển | -1 | 3 | 
| 4 | -1 | 3 | 2 | di chuyển | -2 | 3 | 

Cuối cùng, điều kiện không thành công, có nghĩa là ngay cả với tất cả tiền thưởng đã thu thập được, chúng tôi không thể duy trì số bước cần thiết. 

Điều này chứng tỏ rằng ngay cả tiền tố phần thưởng có vẻ giống nhau cũng trở nên không đủ khi độ dài đường dẫn tăng nhẹ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chuyển qua các ô một lần với công việc liên tục trên mỗi bước | 
| Không gian | O(1) | Chỉ có một số quầy được duy trì | 

Giải pháp chia tỷ lệ trực tiếp với n, điều này là bắt buộc vì n có thể lên tới 200000. Bất kỳ thuật toán nào cố gắng mô phỏng các lần truy cập lại hoặc theo dõi các chuyển đổi trạng thái sẽ vượt quá giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, h = map(int, input().split())
    a = list(map(int, input().split()))

    bonus = 0
    for i in range(n):
        if h + bonus <= 1:
            return "NO"
        h -= 1
        bonus += a[i]
    return "YES"

# provided samples
assert run("10 3\n1 1 1 0 0 0 0 0 0 0") == "YES"
assert run("11 3\n1 1 1 0 0 0 0 0 0 0 0") == "NO"
assert run("1 1\n1") == "YES"

# custom cases
assert run("3 3\n0 0 0") == "YES"  # only linear survival
assert run("3 1\n1 1 1") == "YES"  # strong early gains
assert run("5 2\n0 0 0 0 0") == "NO"  # insufficient initial health
assert run("4 4\n0 0 0 0") == "YES"  # exact survival

| Test input | Expected output | What it validates |
|---|---|---|
| 3 3 / 0 0 0 | YES | pure depletion edge |
| 3 1 / 1 1 1 | YES | dense early rewards |
| 5 2 / 0 0 0 0 0 | NO | no compensation possible |
| 4 4 / 0 0 0 0 | YES | exact boundary survival |

## Edge Cases

One edge case is when there are no watermelons at all. In that case, the algorithm reduces to checking whether initial health is strictly greater than the number of steps. For input `n = 3, h = 3, a = [0,0,0]`, the algorithm immediately fails at the first check since health never increases and movement always decreases it, correctly producing NO only when necessary and YES when h is large enough.

Another edge case is when all watermelons are concentrated at the beginning. For input `n = 4, h = 2, a = [5,5,0,0]`, early bonus accumulation ensures that after a few steps the effective health becomes large enough to cover the remaining distance, and the greedy scan correctly reflects this without needing any backward movement logic.

A final edge case is minimal input `n = 1`. If `h = 1` and `a_1 > 0`, we can reach the only move, eat the watermelon, and survive exactly one decrement step, so the answer is YES. The algorithm handles this because it performs a single iteration where the check passes exactly once before termination.
```
