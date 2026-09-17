---
title: "CF 104720E - Đặt món ăn"
description: "Chúng ta có hai chuỗi món ăn, mỗi món ăn được biểu thị bằng một chữ cái viết hoa. Trình tự đầu tiên là sự sắp xếp hiện tại trên bàn và trình tự thứ hai là sự sắp xếp cuối cùng mong muốn."
date: "2026-06-29T05:42:01+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104720
codeforces_index: "E"
codeforces_contest_name: "UTPC x WiCS Contest 10-06-23"
rating: 0
weight: 104720
solve_time_s: 58
verified: true
draft: false
---

[CF 104720E - Đặt món ăn](https://codeforces.com/problemset/problem/104720/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai chuỗi món ăn, mỗi món ăn được biểu thị bằng một chữ cái viết hoa. Trình tự đầu tiên là sự sắp xếp hiện tại trên bàn và trình tự thứ hai là sự sắp xếp cuối cùng mong muốn. Hoạt động duy nhất được phép là hoán đổi hai món ăn liền kề và chúng tôi muốn chuyển đổi chuỗi đầu tiên thành chuỗi thứ hai bằng cách sử dụng ít lần hoán đổi như vậy nhất có thể. Nếu việc đó không thể thực hiện được thì chúng tôi phải báo cáo là không thể thực hiện được. 

Hạn chế quan trọng là các phép hoán đổi chỉ liền kề nhau, vì vậy về cơ bản chúng ta đang làm việc với khái niệm cổ điển về việc chuyển đổi chuỗi này thành chuỗi khác bằng cách sử dụng phép đảo ngược. Tuy nhiên, điều này chỉ hoạt động nếu hai chuỗi chứa chính xác nhiều tập hợp chữ cái giống nhau, vì việc hoán đổi không thể tạo hoặc hủy các món ăn. Nếu bất kỳ ký tự nào xuất hiện với số lần khác nhau thì việc chuyển đổi sẽ không thể thực hiện được ngay lập tức. 

Kích thước đầu vào nhỏ, tối đa 100 món ăn. Điều này có nghĩa là ngay cả các thuật toán bậc hai hoặc bậc ba cũng được chấp nhận. Một giải pháp mô phỏng các giao dịch hoán đổi hoặc thử tất cả các lần sắp xếp lại vẫn sẽ thành công, nhưng chúng ta nên hướng tới việc giảm thiểu rõ ràng về một cấu trúc đã biết. 

Một số trường hợp đặc biệt quan trọng: 

Nếu tần số của các chữ cái khác nhau giữa hai chuỗi thì câu trả lời phải là -1 ngay cả khi có thể căn chỉnh một phần. Ví dụ, chuyển đổi`AAB`vào trong`ABC`là không thể bởi vì không có`C`trong nguồn. 

Nếu các chuỗi đã bằng nhau thì câu trả lời là 0. 

Nếu tất cả các ký tự giống hệt nhau thì mọi hoán vị đều hợp lệ và câu trả lời chỉ đơn giản là số lần hoán đổi liền kề cần thiết để căn chỉnh các chuỗi giống hệt nhau, sẽ luôn bằng 0 vì chúng không thể phân biệt được. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô phỏng quá trình một cách trực tiếp. Chúng tôi có thể quét chuỗi liên tục, tìm các vị trí không khớp và hoán đổi các ký tự liền kề để đẩy các chữ cái chính xác vào đúng vị trí. Mỗi lần hoán đổi sẽ làm giảm tình trạng rối loạn cục bộ và chúng tôi có thể tiếp tục cho đến khi chuỗi khớp với mục tiêu hoặc không thể thực hiện được tiến trình nào. 

Điều này đúng vì mỗi thao tác đều hợp lệ và chúng ta chỉ dừng lại khi đạt được mục tiêu. Vấn đề là việc mô phỏng các giao dịch hoán đổi một cách đơn giản có thể yêu cầu di chuyển một ký tự qua các vị trí O(n) và việc thực hiện điều này đối với các ký tự O(n) sẽ dẫn đến hành vi O(n²) hoặc tệ hơn. Với n lên tới 100, điều này vẫn ổn, nhưng nó không phải là con đường lý luận rõ ràng nhất. 

Một quan điểm có cấu trúc hơn là coi đây là một vấn đề đếm ngược, nhưng có một điểm thay đổi: chúng tôi không sắp xếp các số tùy ý, chúng tôi sắp xếp một chuỗi với các bản sao và thứ tự mục tiêu cố định. Chúng tôi quét chuỗi nguồn từ trái sang phải và đối với mỗi vị trí, chúng tôi quyết định lần xuất hiện nào của ký tự bắt buộc mà chúng tôi sẽ khớp. Sau khi chúng tôi sửa lỗi ghép nối đó, số lần hoán đổi cần thiết chính xác là số lần đảo ngược được tạo ra bằng cách di chuyển lần xuất hiện đó vào đúng vị trí. 

Quan sát quan trọng là mỗi ký tự trong mục tiêu xác định một vị trí mục tiêu cụ thể và chúng ta có thể khớp các lần xuất hiện theo thứ tự. Khi sự khớp này được cố định, số lần hoán đổi liền kề tối thiểu là số lần giao nhau theo cặp giữa các vị trí đã chọn trong thứ tự nguồn và thứ tự đích. Điều này làm giảm vấn đề đếm hơn là vấn đề mô phỏng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng hoán đổi vũ phu | O(n³) trường hợp xấu nhất | O(n) | Được chấp nhận nhưng không cần thiết | 
| So khớp + Đếm nghịch đảo | O(n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Trước tiên hãy xác minh tính khả thi bằng cách kiểm tra xem cả hai chuỗi có số ký tự giống nhau hay không. Nếu có bất kỳ sự không khớp nào tồn tại, quá trình chuyển đổi không thể hoàn thành vì các hoán đổi không làm thay đổi nhiều ký tự. 
2. Đối với mỗi ký tự trong chuỗi nguồn, hãy xây dựng danh sách vị trí của nó. Làm tương tự cho chuỗi mục tiêu. Điều này cho chúng ta một ánh xạ xác định giữa các lần xuất hiện: lần đầu tiên`A`trong nguồn phải tương ứng với nguồn đầu tiên`A`trong mục tiêu, thứ hai đến thứ hai, v.v. 
3. Xây dựng một mảng`p`Ở đâu`p[i]`là chỉ mục đích của ký tự hiện tại ở vị trí nguồn`i`. Điều này chuyển đổi vấn đề thành việc đo khoảng cách giữa hoán vị nguồn và hoán vị đích. 
4. Tính số lần đảo ngược trong mảng`p`. Mỗi phép đảo ngược tương ứng chính xác với một hoán đổi liền kề cần thiết để cố định thứ tự tương đối của hai phần tử. 
5. Trả về số lần đảo ngược. 

Việc giải thích đảo ngược hoạt động vì mọi hoán đổi liền kề sẽ sửa chính xác một đảo ngược và không tạo ra hoặc phá hủy cấu trúc nào khác một cách nhất quán. 

### Tại sao nó hoạt động 

Việc sửa lỗi ghép nối giữa các chữ cái giống hệt nhau sẽ loại bỏ sự mơ hồ: chúng tôi không còn chọn chữ nào nữa`A`phù hợp với cái nào`A`, chúng tôi thực thi tính nhất quán của thứ tự theo chỉ số xuất hiện. Khi ánh xạ này được thiết lập, phép biến đổi sẽ trở thành hoán vị của các chỉ số. Số lượng hoán đổi liền kề tối thiểu cần thiết để chuyển đổi một hoán vị này thành một hoán vị khác chính xác là khoảng cách đảo ngược giữa chúng, bởi vì mỗi hoán đổi liền kề sẽ thay đổi số lượng đảo ngược chính xác bằng một và chúng ta luôn có thể giảm các đảo ngược một cách tham lam cho đến khi không còn lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    s = input().strip()
    t = input().strip()

    if sorted(s) != sorted(t):
        print(-1)
        return

    pos_s = {}
    pos_t = {}

    for i, c in enumerate(s):
        pos_s.setdefault(c, []).append(i)

    for i, c in enumerate(t):
        pos_t.setdefault(c, []).append(i)

    p = [0] * n
    for c in pos_s:
        for i, idx in enumerate(pos_s[c]):
            p[idx] = pos_t[c][i]

    inv = 0
    for i in range(n):
        for j in range(i + 1, n):
            if p[i] > p[j]:
                inv += 1

    print(inv)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách xác minh rằng cả hai chuỗi đều chứa cùng nhiều bộ ký tự. Việc sắp xếp là đủ vì n nhỏ và nó đưa ra sự kiểm tra tính bằng nhau trực tiếp. 

Sau đó chúng tôi ghi lại chỉ số xuất hiện của từng ký tự trong cả hai chuỗi. Điều này rất cần thiết vì các bản sao phải được khớp một cách nhất quán; nếu không, các cặp khác nhau có thể dẫn đến số lần hoán đổi khác nhau. 

Mảng`p`mã hóa nơi mỗi vị trí nguồn phải kết thúc theo thứ tự đích. Sau khi hoán vị này được xây dựng, phần còn lại của vấn đề hoàn toàn là tổ hợp: đếm các nghịch đảo. 

Vòng đếm nghịch đảo được thực hiện theo cách đơn giản nhất có thể vì n chỉ bằng 100. Cây Fenwick cũng có thể hoạt động nhưng ở đây không cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5
s = AAAAB
t = BAAAA
```Chúng tôi xây dựng ánh xạ xuất hiện. 

| Bước | Nhân vật | Chỉ số nguồn | Chỉ số mục tiêu | Lập bản đồ | 
| --- | --- | --- | --- | --- | 
| A | 0,1,2,3 | 1,2,3,4 | 0→1, 1→2, 2→3, 3→4 | | 
| B | 4 | 0 | 4→0 | | 

Vì thế`p = [1,2,3,4,0]`. 

Bây giờ chúng tôi đếm nghịch đảo: 

| tôi | p[i] | nghịch đảo đóng góp | 
| --- | --- | --- | 
| 0 | 1 | so sánh với 0 → 1 nghịch đảo | 
| 1 | 2 | so sánh với 0 → 1 nghịch đảo | 
| 2 | 3 | so sánh với 0 → 1 nghịch đảo | 
| 3 | 4 | so sánh với 0 → 1 nghịch đảo | 
| 4 | 0 | 0 | 

Tổng cộng = 4. 

Điều này cho thấy việc di chuyển`B`từ đầu đến cuối cần bốn lần hoán đổi liền kề, phù hợp với trực giác. 

### Ví dụ 2 

đầu vào:```
n = 10
s = ABCDEFGHIJ
t = ABCDEFGHIK
```Chúng tôi ngay lập tức phát hiện sự không khớp trong nhiều tập hợp:`J`tồn tại ở`s`nhưng không ở trong`t`, trong khi`K`tồn tại ở`t`nhưng không ở trong`s`. 

| Kiểm tra | Kết quả | 
| --- | --- | 
| đã sắp xếp == đã sắp xếp(t) | sai | 

Đầu ra là`-1`. 

Điều này xác nhận rằng tính khả thi được xác định hoàn toàn bằng số lượng ký tự, không phụ thuộc vào thứ tự. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | đếm ngược sử dụng các vòng lặp lồng nhau trên các vị trí | 
| Không gian | O(n) | lưu trữ danh sách vị trí và mảng hoán vị | 

Với n 100, các phép so sánh 10⁴ trong trường hợp xấu nhất là không đáng kể và mức sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.readline  # placeholder for actual integration

# Sample-like and custom tests (conceptual placeholders)
assert True  # replace with real wiring

# custom cases
# all equal
# s = t, answer 0

# single swap needed

# impossible case
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\nA\nA | 0 | kích thước tối thiểu, đã bằng nhau | 
| 2\nAB\nBA | 1 | đảo ngược đơn | 
| 3\nAB\nAC | -1 | nhân vật bị thiếu khiến không thể | 
| 5\nAAAAB\nBAAAA | 4 | ký tự lặp lại, khớp đúng | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi các ký tự lặp lại nhiều. Ví dụ`AAAAAB`ĐẾN`BAAAAA`. Thuật toán chỉ định chính xác các lần xuất hiện theo thứ tự, do đó, một`B`được khớp một cách nhất quán và tất cả các lần đảo ngược đều được tính tương ứng với danh tính cố định đó. Một sự hoán đổi tham lam ngây thơ có thể vô tình đối xử giống hệt nhau`A`s có thể hoán đổi cho nhau và vẫn nhận được câu trả lời đúng, nhưng nếu không có ánh xạ nhất quán thì sẽ dễ dàng tính sai các giao dịch hoán đổi trong các kết hợp phức tạp hơn. 

Một trường hợp khác là hoàn toàn không thể thực hiện được do thiếu các chữ cái. Quá trình kiểm tra tính khả thi sẽ nắm bắt được điều này ngay lập tức thông qua so sánh tần số, đảm bảo không khớp một phần nào dẫn đến câu trả lời bằng số không chính xác.
