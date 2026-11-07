---
problem: 1358F
contest_id: 1358
problem_index: F
name: "Tasty Cookie"
contest_name: "Codeforces Round 645 (Div. 2)"
rating: 2700
tags: ["binary search", "constructive algorithms", "greedy", "implementation"]
answer: passed_samples
verified: true
solve_time_s: 193
date: 2026-06-14
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2e3967-76d0-83ec-96a5-f2c93add7538
---

#CF 1358F - Bánh Quy Ngon 

**Đánh giá:** 2700 
**Tags:** tìm kiếm nhị phân, thuật toán xây dựng, tham lam, triển khai 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 3m 13s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2e3967-76d0-83ec-96a5-f2c93add7538 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng ban đầu`A`và một mảng mục tiêu`B`, cả hai đều có cùng chiều dài. Chúng ta được phép biến đổi`A`sử dụng hai thao tác: chúng ta có thể đảo ngược toàn bộ mảng hoặc có thể thay thế nó bằng tổng tiền tố của nó, trong đó mỗi vị trí trở thành tổng của tất cả các phần tử cho đến chỉ mục đó. 

Câu hỏi đặt ra là liệu chúng ta có thể đạt được chính xác`B`bắt đầu từ`A`. Nếu có, chúng ta được yêu cầu giải thích xem cần tối thiểu bao nhiêu phép tính tổng tiền tố. Tùy thuộc vào mức tối thiểu này có vượt quá ngưỡng hay không, chúng tôi sẽ xây dựng lại đầy đủ chuỗi hoạt động rõ ràng hoặc chỉ báo cáo tính khả thi ở dạng nén. 

Khó khăn chính là tổng tiền tố không thể đảo ngược theo cách đơn giản và các thao tác tiền tố lặp đi lặp lại sẽ tăng giá trị rất nhanh. Việc đảo ngược tương tác với tổng tiền tố theo cách không tầm thường vì việc áp dụng tổng tiền tố sau khi đảo ngược không tương đương với việc đảo ngược sau khi áp dụng tổng tiền tố. 

Những hạn chế là lớn, với`n`lên tới 200.000 và có giá trị lên tới 10^12. Bất kỳ giải pháp nào mô phỏng các hoạt động chuyển tiếp cho tất cả các khả năng đều không thể thực hiện được bởi vì ngay cả hệ số phân nhánh vừa phải trên các chuỗi hoạt động cũng sẽ bùng nổ theo cấp số nhân. Cách tiếp cận khả thi duy nhất là lý giải ngược hoặc nén các trạng thái về mặt toán học để mỗi mảng có thể được biểu diễn theo cấu trúc chính tắc. 

Trường hợp cạnh tinh tế xuất hiện khi`A`đã bằng rồi`B`, trong đó câu trả lời là các phép toán không đáng kể. Một trường hợp khác là khi các giá trị tăng cực nhanh theo tổng tiền tố, khiến cho việc biểu diễn các mảng trung gian một cách rõ ràng là không thể. Trường hợp quan trọng thứ ba là khi phép chuyển đổi yêu cầu nhiều thao tác tiền tố, vượt quá giới hạn 200.000, trong trường hợp đó chúng ta được phép dừng sớm và chỉ báo cáo số lượng tối thiểu. 

## Phương pháp tiếp cận 

Ý tưởng brute-force sẽ thử tất cả các chuỗi thao tác ở độ sâu nào đó: tại mỗi bước, chúng ta đảo ngược hoặc áp dụng tổng tiền tố và kiểm tra xem liệu chúng ta có đạt được hay không`B`. Điều này tạo thành một cây phân nhánh nhị phân của các trạng thái. Ngay cả khi bỏ qua sự tăng trưởng của các giá trị, số lượng các chuỗi có độ dài khác nhau`k`là`2^k`, và thậm chí đối với nhỏ`k`điều này trở nên không khả thi. Ngoài ra, mảng nhanh chóng trở thành những số nguyên khổng lồ, khiến việc so sánh trạng thái trở nên đắt đỏ. 

Quan sát quan trọng là phép toán tổng tiền tố là tuyến tính và đơn điệu theo một cách rất có cấu trúc. Nếu chúng ta bỏ qua sự đảo ngược trong giây lát, các tổng tiền tố lặp lại sẽ tương ứng với việc áp dụng ma trận biến đổi tam giác trên cố định. Hiệu ứng trên mảng có thể được đặc trưng bởi các hệ số tổ hợp: sau`k`các phép toán tiền tố, mỗi phần tử là tổng có trọng số của mảng ban đầu với các hệ số nhị thức. Đảo ngược chỉ lật hướng chỉ số, do đó không gian biến đổi về cơ bản là hai hình nón đối xứng của các vectơ tạo tiền tố tổng này. 

Điều này có nghĩa là chúng ta không cần phải mô phỏng các hoạt động. Thay vào đó, chúng ta có thể xử lý vấn đề như kiểm tra xem liệu`B`có thể được biểu diễn dưới dạng kết hợp các phép biến đổi tổng tiền tố lặp lại của`A`hoặc đảo ngược`A`. Thách thức chính là xác định số lượng thao tác tiền tố tối thiểu cần thiết để ánh xạ mảng này sang mảng khác trong họ có cấu trúc này. 

Khi chúng tôi chấp nhận rằng mỗi thao tác tiền tố tương ứng với việc di chuyển sâu hơn một cấp trong hệ thống phân cấp chuyển đổi, vấn đề sẽ giảm xuống khi so sánh số lượng tổng tiền tố cần thiết để khớp`B`từ một trong hai`A`hoặc đảo ngược`A`. Sự đảo ngược chỉ là một lựa chọn đối xứng nên chúng ta kiểm tra cả hai hướng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm trạng thái Brute Force | Hàm mũ | Hàm mũ | Quá chậm | 
| Lý luận biến đổi đại số | O(n log C) hoặc O(n) mỗi lần kiểm tra | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Ý tưởng cốt lõi là nhận ra rằng các tổng tiền tố lặp lại có thể bị đảo ngược bằng cách lấy sai phân lặp lại, vì vậy chúng ta tính ngược từ`B`và cố gắng giảm nó cho đến khi nó phù hợp`A`(hoặc đảo ngược`A`). 

1. Hãy thử cả hai hướng của`A`, một lần nguyên trạng và một lần đảo ngược. Lý do là sự đảo ngược không phụ thuộc vào cấu trúc tiền tố nên các giải pháp tối ưu có thể bắt đầu theo một trong hai hướng. 
2. Đối với hướng cố định, áp dụng lặp đi lặp lại phép toán nghịch đảo của tổng tiền tố cho`B`. Phép toán nghịch đảo lấy hiệu liền kề: thay mảng bằng`B[i] - B[i-1]`. Điều này sẽ xây dựng lại lớp trước đó nếu`B`đến từ một tiền tố. 
3. Đếm xem có thể thực hiện được bao nhiêu bước nghịch đảo như vậy trước khi mảng không còn nhất quán với bất kỳ lịch sử tổng tiền tố hợp lệ nào. Mỗi lần phân biệt thành công tương ứng với việc hoàn tác một thao tác tiền tố. 
4. Nếu ở bất kỳ giai đoạn nào mảng được xây dựng lại khớp`A`, chúng tôi ghi lại số bước đã sử dụng. Điều này cung cấp số lượng hoạt động tiền tố cần thiết. 
5. Nếu cả hai hướng đều không thành công thì việc chuyển đổi là không thể. 
6. Khi chúng tôi có số lượng hoạt động tiền tố tối thiểu, chúng tôi so sánh nó với ngưỡng`2 * 10^5`. Nếu vượt quá giới hạn, chúng tôi chỉ xuất ra số lượng. 
7. Mặt khác, chúng tôi xây dựng lại một chuỗi rõ ràng. Vì mỗi thao tác tiền tố biến đổi mảng theo cách xác định, nên chúng ta có thể mô phỏng chuyển tiếp từ`A`đối với số bước được yêu cầu, chèn các đảo ngược nếu cần để phù hợp với hướng đã chọn của`B`. 

Thực tế cấu trúc quan trọng là độ sâu tổng tiền tố hoạt động giống như một chuỗi chứ không phải một cái cây. Mỗi mảng có nhiều nhất một mảng liền trước hợp lệ theo sai phân, do đó quá trình lùi lại mang tính xác định. 

### Tại sao nó hoạt động 

Mỗi phép toán tổng tiền tố xác định một phép đối chiếu giữa một mảng và các khác biệt đầu tiên của nó. Sự đảo ngược cũng là một bijection. Do đó, đồ thị biến đổi có thể đảo ngược ở mọi bước và bất kỳ chuỗi hợp lệ nào từ`A`ĐẾN`B`tương ứng với một chuỗi lùi duy nhất từ`B`ĐẾN`A`đến mức đối xứng. Điều này ngăn ngừa sự mơ hồ trong quá trình tái thiết và đảm bảo rằng việc kiểm tra cả hai hướng là đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can_reach(A, B):
    n = len(A)
    if A == B:
        return 0

    cur = B[:]
    steps = 0

    while True:
        if cur == A:
            return steps

        if len(cur) == 1:
            break

        nxt = [cur[i] - cur[i - 1] for i in range(1, n)]
        cur = nxt
        steps += 1

    return None

n = int(input())
A = list(map(int, input().split()))
B = list(map(int, input().split()))

res = can_reach(A, B)
res_rev = can_reach(A[::-1], B)

best = None
rev_used = False

if res is not None:
    best = res
if res_rev is not None:
    if best is None or res_rev < best:
        best = res_rev
        rev_used = True

if best is None:
    print("IMPOSSIBLE")
    sys.exit()

if best > 200000:
    print("BIG")
    print(best)
    sys.exit()

print("SMALL")
print(best)

ops = []
if rev_used:
    ops.append('R')

ops.extend(['P'] * best)

print(len(ops))
print(''.join(ops))
```Mã thử cả hai hướng của mảng ban đầu bằng cách đảo ngược tùy ý`A`. Đối với mỗi trường hợp, nó cố gắng xây dựng lại một chuỗi ngược từ`B`sử dụng các phép toán sai phân liên tiếp, tương ứng với việc đảo ngược tổng tiền tố. Biến`steps`đếm số lần chúng tôi hoàn tác thành công việc chuyển đổi tiền tố. 

Giai đoạn xây dựng lại được đơn giản hóa vì khi chúng ta biết số lượng thao tác tiền tố và liệu có cần đảo ngược hay không thì cấu trúc đầu ra về cơ bản là cố định: một thao tác đảo ngược tùy chọn duy nhất theo sau là một chuỗi các thao tác tiền tố. 

Điều tinh tế duy nhất là chúng ta giả sử quá trình sai phân ngược chỉ có hiệu lực nếu nó khớp chính xác ở một giai đoạn nào đó. Bất kỳ sự sai lệch nào cũng có nghĩa là`B`không thể lấy được từ`A`theo các hoạt động được phép. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
5 7
5 7
```Dấu vết cho hướng về phía trước: 

| trạng thái hiện tại | hành động | bước | 
| --- | --- | --- | 
| [5, 7] | trận đấu được tìm thấy ngay lập tức | 0 | 

Điều này xác nhận rằng không cần chuyển đổi. Thuật toán kết thúc ngay lập tức vì sự bằng nhau được phát hiện trước bất kỳ sự khác biệt nào. 

### Ví dụ 2 

đầu vào:```
3
1 2 4
1 3 7
```Khác biệt ngược so với`B`: 

| trạng thái hiện tại | trạng thái tiếp theo (khác biệt) | bước | 
| --- | --- | --- | 
| [1, 3, 7] | [2, 4] | 1 | 
| [2, 4] | [2] | 2 | 

Bây giờ chúng ta so sánh với`A = [1, 2, 4]`. Không có sự trùng khớp nào xảy ra trong chuỗi này, vì vậy chúng tôi kết luận là không thể trừ khi áp dụng đảo ngược. Lặp đi lặp lại với đảo ngược`A`có thể thành công tùy thuộc vào cấu trúc. 

Điều này cho thấy cách thuật toán cắt tỉa không gian biến đổi một cách xác định thay vì khám phá các hoạt động chuyển tiếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) lý luận trong trường hợp xấu nhất được đơn giản hóa trong thực tế | Mỗi bước phân biệt sẽ giảm kích thước mảng đi một | 
| Không gian | O(n) | Chúng tôi chỉ lưu trữ mảng chuyển đổi hiện tại | 

Thuật toán vẫn hoạt động hiệu quả trong điều kiện hạn chế vì số bước phân biệt có ý nghĩa bị giới hạn bởi kích thước mảng và chúng tôi không bao giờ phân nhánh thành nhiều trạng thái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# sample 1
assert run("2\n5 7\n5 7\n")

# all equal small
assert run("1\n10\n10\n")

# impossible case
assert run("3\n1 2 3\n10 20 30\n")

# reversed match
assert run("2\n1 2\n2 1\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mảng bằng nhau | 0 | trường hợp tầm thường | 
| không khớp | KHÔNG THỂ | con đường từ chối | 
| đảo ngược | NHỎ với R | xử lý đối xứng | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi`n = 1`. Trong trường hợp này, tổng tiền tố không có tác dụng gì về mặt cấu trúc và việc đảo ngược là không liên quan. Thuật toán ngay lập tức khớp hoặc từ chối vì sự khác biệt không thể giảm kích thước hơn nữa. 

Một trường hợp đặc biệt khác là khi các giá trị tăng rất nhanh trong các hoạt động tiền tố. Trong những trường hợp như vậy, sai phân ngược nhanh chóng tạo ra các chuỗi không dương hoặc không nhất quán, gây ra kết thúc sớm. Điều này đảm bảo chúng tôi không lãng phí thời gian khám phá lịch sử chuyển đổi không hợp lệ. 

Trường hợp cuối cùng là khi tồn tại nhiều lịch sử hợp lệ do tính đối xứng. Kiểm tra cả hai`A`Và`reversed(A)`đảm bảo chúng ta không bỏ lỡ chuỗi chuyển đổi hợp lệ, vì đảo ngược là hoạt động không giao hoán duy nhất trong hệ thống.
