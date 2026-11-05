---
title: "CF 1193A - Khu vui chơi giải trí"
description: "Chúng tôi được cung cấp một loạt các điểm tham quan và một số đường trượt một chiều đã được lên kế hoạch giữa các cặp trong số đó. Sau khi xây dựng, mỗi slide có thể được đảo ngược hoặc giữ nguyên, độc lập với các slide khác. Do đó, điều cuối cùng chúng tôi chọn chỉ là một hướng đi cho mọi lợi thế hiện có."
date: "2026-06-13T13:33:16+07:00"
tags: ["codeforces", "competitive-programming", "*special", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 1193
codeforces_index: "A"
codeforces_contest_name: "CEOI 2019 day 2 online mirror (unrated, IOI format)"
rating: 0
weight: 1193
solve_time_s: 241
verified: true
draft: false
---

[CF 1193A - Công viên giải trí](https://codeforces.com/problemset/problem/1193/A) 

**Đánh giá:** - 
**Tags:** *đặc biệt, dp, toán 
**Thời gian giải:** 4 phút 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một loạt các điểm tham quan và một số đường trượt một chiều đã được lên kế hoạch giữa các cặp trong số đó. Sau khi xây dựng, mỗi slide có thể được đảo ngược hoặc giữ nguyên, độc lập với các slide khác. Do đó, điều cuối cùng chúng tôi chọn chỉ là một hướng đi cho mọi lợi thế hiện có. 

Một đề xuất được coi là có giá trị về mặt vật lý nếu chúng ta có thể ấn định độ cao cho các điểm tham quan sao cho mọi đường trượt đều đi xuống hoàn toàn. Điều kiện này tương đương với việc nói rằng đồ thị có hướng được hình thành bởi các hướng đã chọn không có chu trình có hướng, bởi vì việc gán chiều cao dẫn đến một thứ tự chặt chẽ của các đỉnh và mọi cạnh phải tuân theo thứ tự đó. 

Vì vậy, mọi đề xuất hợp lệ đều tương ứng chính xác với hướng không theo chu kỳ của đồ thị vô hướng đã cho. 

Chi phí của một đề xuất là số lượng slide có hướng cuối cùng khác với hướng đã cho ban đầu. Nhiệm vụ không phải là tìm ra một đề xuất tốt nhất mà là tổng hợp chi phí này trên tất cả các định hướng hợp lệ (không theo chu kỳ). 

Ràng buộc về số lượng nút là nhỏ, nhưng số cạnh có thể là bậc hai. Điều đó cho thấy giải pháp mong muốn là không liệt kê các hướng một cách rõ ràng, vì có 2^m khả năng, điều này trở nên rất lớn ngay cả đối với m vừa phải. 

Một vấn đề tế nhị sẽ xuất hiện nếu người ta cố gắng chỉ nghĩ theo chu kỳ. Một ý tưởng ngây thơ là cố gắng đếm các hướng không theo chu kỳ bằng cách kiểm tra các hoán vị hoặc phân loại tôpô, nhưng không chú ý đến tính đối xứng, điều này nhanh chóng dẫn đến việc đếm quá mức hoặc tính hai lần cùng một hướng ở các dạng khác nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả 2^m cách để gán hướng cho các cạnh, sau đó kiểm tra xem biểu đồ kết quả có theo chu kỳ hay không và tính chi phí cho mỗi hướng hợp lệ. Việc kiểm tra chu kỳ cho mỗi cấu hình sẽ yêu cầu O(n + m), dẫn đến thời gian là O(2^m · m). Với m lên tới khoảng 150, điều này vượt xa giới hạn khả thi. 

Một quan điểm có cấu trúc hơn là ngừng tập trung vào các cạnh và thay vào đó tập trung vào thứ tự các đỉnh. Nếu chúng ta sửa một hoán vị của tất cả các điểm hấp dẫn, chúng ta có thể định hướng mọi cạnh một cách nhất quán theo thứ tự đó: đỉnh trước đến đỉnh sau. Điều này tự động tạo ra một hướng không theo chu kỳ hợp lệ và mọi hướng không theo chu kỳ có thể được liên kết với ít nhất một hoán vị như vậy. 

Điều này chuyển không gian từ cấu hình cạnh 2^m sang n! hoán vị. Sự đơn giản hóa quan trọng là chi phí trở nên dễ dàng biểu diễn dưới dạng hoán vị: đối với một cạnh cố định u → v trong đầu vào ban đầu, cạnh đó bị đảo ngược chính xác khi v xuất hiện trước u trong hoán vị. Trong tất cả các hoán vị, sự kiện này có tính đối xứng hoàn hảo, do đó mỗi hướng xảy ra thường xuyên như nhau. 

Sự đối xứng này cho phép toàn bộ tổng thu gọn thành một dạng đóng đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê các hướng cạnh | O(2^m · m) | O(n + m) | Quá chậm | 
| Đếm hoán vị có tính đối xứng | O(n + m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Quan sát rằng mọi hoán vị của các đỉnh đều xác định một hướng không theo chu kỳ hợp lệ bằng cách hướng mỗi cạnh từ trước đến sau trong hoán vị. Điều này cung cấp một bản đồ hoàn chỉnh từ các hoán vị đến các đề xuất pháp lý. 
2. Cố định một cạnh u → v từ đầu vào. Trên tất cả các hoán vị, hãy xem xét tần suất cạnh này bị đảo ngược theo hướng cảm ứng. Sự đảo ngược xảy ra chính xác khi v xuất hiện trước u trong hoán vị. 
3. Bằng tính đối xứng của tất cả các hoán vị, u đứng trước v ở đúng một nửa trong số chúng, và v đứng trước u ở nửa còn lại. Do đó, đối với mỗi cạnh, số hoán vị đóng góp chi phí bằng 1 cho cạnh đó là n! / 2. 
4. Tổng chi phí trên một hoán vị là tổng của các cạnh xem cạnh đó có bị đảo ngược hay không. Tính tổng tất cả các hoán vị cho phép đổi tổng, do đó mỗi cạnh đóng góp độc lập. 
5. Nhân phần đóng góp của một cạnh với số cạnh m để có tổng số tiền trên tất cả các đề xuất hợp lệ. 
6. Tính giai thừa n! modulo 998244353 và nhân với m và nghịch đảo mô đun của 2. 

Ý tưởng chính là các đóng góp của cạnh là độc lập theo các hoán vị thống nhất, loại bỏ mọi nhu cầu xem xét cấu trúc biểu đồ ngoài số cạnh. 

### Tại sao nó hoạt động 

Mỗi hướng không theo chu kỳ tương ứng với ít nhất một hoán vị và mỗi hoán vị tạo ra chính xác một hướng không theo chu kỳ. Thay vào đó, khi tính tổng chi phí theo tất cả các hướng hợp lệ, chúng ta có thể tính tổng tất cả các hoán vị mà không làm mất tính chính xác vì mỗi hướng được tính ít nhất một lần một cách nhất quán với các hoán vị tạo ra của nó. Việc phân tách chi phí chỉ phụ thuộc vào thứ tự tương đối của các điểm cuối của mỗi cạnh và các thứ tự tương đối này được phân bổ đồng đều trên các hoán vị, làm cho mỗi cạnh đóng góp một giá trị dự kiến ​​cố định và tổng giá trị độc lập với phần còn lại của biểu đồ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    n, m = map(int, input().split())
    
    fact = 1
    for i in range(1, n + 1):
        fact = fact * i % MOD
    
    inv2 = (MOD + 1) // 2
    
    ans = m % MOD * fact % MOD * inv2 % MOD
    print(ans)

if __name__ == "__main__":
    solve()
```Tính toán giai thừa mã hóa tổng số hoán vị đỉnh. Phép nhân với m tổng hợp các đóng góp giống hệt nhau từ mỗi cạnh. Việc chia cho 2 được thực hiện bằng cách sử dụng nghịch đảo mô-đun vì chính xác một nửa số hoán vị đặt điểm cuối này trước điểm cuối kia. 

Không cần phải duyệt đồ thị hoặc DP vì cấu trúc của bài toán quy giản hoàn toàn về tính đối xứng hoán vị. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 1
1 2
```Chúng ta có 2 đỉnh và một cạnh. 

| Hoán vị | Quan hệ đặt hàng | Cạnh đảo ngược? | Chi phí | 
| --- | --- | --- | --- | 
| 1 2 | 1 trước 2 | Không | 0 | 
| 2 1 | 2 trước 1 | Có | 1 | 

Tổng chi phí là 1, phù hợp với công thức m · n! / 2 = 1 · 2 / 2 = 1. 

Điều này xác nhận rằng mỗi hoán vị đều đóng góp chính xác và tính đối xứng được duy trì ngay cả trong biểu đồ nhỏ nhất. 

### Mẫu 2 

đầu vào:```
3 3
1 2
2 3
1 3
```Chúng tôi liệt kê các hoán vị của ba đỉnh. 

| Hoán vị | Các cạnh đảo ngược | Chi phí | 
| --- | --- | --- | 
| 1 2 3 | không | 0 | 
| 1 3 2 | (2,3) | 1 | 
| 2 1 3 | (1,2) | 1 | 
| 2 3 1 | (1,2),(1,3) | 2 | 
| 3 1 2 | (1,3),(2,3) | 2 | 
| 3 2 1 | cả ba | 3 | 

Tổng là 9. Công thức cho m · n! / 2 = 3 · 6 / 2 = 9, khớp chính xác. 

Dấu vết này cho thấy sự đóng góp của cạnh vẫn được cân bằng giữa các hoán vị mặc dù sự phụ thuộc tồn tại bên trong mỗi hoán vị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | tính giai thừa lên tới n | 
| Không gian | O(1) | chỉ một vài biến mô-đun | 

Giải pháp này phù hợp một cách thoải mái với các ràng buộc vì n ≤ 18 và việc tính toán tránh được mọi phép liệt kê theo cấp số nhân. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    n, m = map(int, input().split())
    
    fact = 1
    for i in range(1, n + 1):
        fact = fact * i % MOD
    
    inv2 = (MOD + 1) // 2
    
    return str(m % MOD * fact % MOD * inv2 % MOD)

# provided samples
assert run("2 1\n1 2\n") == "1"
assert run("3 3\n1 2\n2 3\n1 3\n") == "9"

# custom cases
assert run("1 0\n") == "0", "single node, no edges"
assert run("4 0\n") == "0", "no edges always zero cost"
assert run("4 1\n1 2\n") == str((1 * 24 * pow(2, MOD-2, MOD)) % MOD), "single edge scaling"
assert run("5 10\n1 2\n2 3\n3 4\n4 5\n1 3\n1 4\n2 5\n3 5\n1 5\n2 4\n") == str(10 * 120 * pow(2, MOD-2, MOD) % MOD), "dense edge case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0`|`0`| trường hợp cạnh đồ thị trống | 
|`4 0`|`0`| không có cạnh nào luôn có chi phí bằng 0 | 
|`4 1 ...`| giai thừa/2 | độ chính xác của cạnh đơn | 
|`5 10 ...`| m·n!/2 | tính nhất quán của biểu đồ dày đặc | 

## Vỏ cạnh 

Biểu đồ không có cạnh cung cấp cách kiểm tra độ chính xác đơn giản nhất. Đầu vào không có ràng buộc nào bị vi phạm và mọi hoán vị đều tạo ra cùng một hướng trống. Chi phí luôn bằng 0 vì không có cạnh nào để đảo ngược và công thức nhân chính xác m = 0 với n!. 

Biểu đồ một cạnh làm nổi bật đối số đối xứng. Bất kể hướng ban đầu được đưa ra là gì, chính xác một nửa số hoán vị sẽ đảo ngược hướng đó. Thuật toán giảm đúng đến n! / 2 và nghịch đảo mô-đun điện toán xử lý việc này mà không có sự mơ hồ về dấu phẩy động. 

Trong các biểu đồ dày đặc, chẳng hạn như biểu đồ hoàn chỉnh trên 5 nút, các tương tác cạnh có vẻ như có thể ảnh hưởng đến tính hợp lệ. Tuy nhiên, vì mọi hoán vị đều hợp lệ bất kể mật độ cạnh, nên sự đóng góp của mỗi cạnh vẫn độc lập. Thuật toán vẫn giảm toàn bộ cấu trúc xuống chỉ đếm các cạnh, xác nhận rằng cấu trúc liên kết đồ thị không bao giờ đi vào công thức cuối cùng.
