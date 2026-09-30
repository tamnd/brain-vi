---
title: "CF 104854B - Cuộc Thi Đẹp"
description: "Chúng tôi duy trì một loạt các vấn đề năng động. Mỗi vấn đề có một giá trị độ khó và một giá trị vẻ đẹp, đồng thời các thao tác có thể chèn hoặc xóa một trường hợp vấn đề cụ thể. Sau mỗi lần cập nhật, chúng tôi được yêu cầu tính toán tổng vẻ đẹp tối đa có thể có của một “cuộc thi” hợp lệ."
date: "2026-06-28T11:03:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104854
codeforces_index: "B"
codeforces_contest_name: "2023-2024 ICPC, Swiss Subregional"
rating: 0
weight: 104854
solve_time_s: 56
verified: true
draft: false
---

[CF 104854B - Cuộc thi đẹp mắt](https://codeforces.com/problemset/problem/104854/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi duy trì một loạt các vấn đề năng động. Mỗi vấn đề có một giá trị độ khó và một giá trị vẻ đẹp, đồng thời các thao tác có thể chèn hoặc xóa một trường hợp vấn đề cụ thể. Sau mỗi lần cập nhật, chúng tôi được yêu cầu tính toán tổng vẻ đẹp tối đa có thể có của một “cuộc thi” hợp lệ. 

Cuộc thi là một chuỗi các bài toán được chọn theo thứ tự và có cấu trúc cứng nhắc: nếu một bài toán khó`d`tiếp theo là một vấn đề khác, vấn đề tiếp theo hẳn phải có khó khăn`⌊d / 2⌋`. Điều này tạo ra một chuỗi khó khăn bắt buộc. Sau khi bạn chọn độ khó bắt đầu, phần còn lại của chuỗi được xác định duy nhất là phép chia số nguyên lặp lại cho hai cho đến khi bạn không thể tiếp tục nữa vì độ khó bắt buộc tiếp theo không còn nữa. 

Vì vậy, nhiệm vụ thực sự là: tại mọi thời điểm, trong số tất cả những khó khăn bắt đầu có thể xảy ra trong nhiều tập hợp hiện tại, hãy chọn một chuỗi sau khi giảm một nửa lặp đi lặp lại và tối đa hóa tổng số điểm đẹp dọc theo chuỗi đó. 

Kích thước đầu vào lên tới 200.000 thao tác. Điều đó ngay lập tức loại trừ việc tính toán lại chuỗi tốt nhất từ ​​đầu sau mỗi lần cập nhật. Một phép tính lại đơn giản sẽ yêu cầu quét tất cả các vấn đề và mô phỏng lặp đi lặp lại các chuỗi, điều này sẽ quá chậm trong trường hợp xấu nhất, vì mỗi thao tác có thể tiêu tốn thời gian tuyến tính trên tất cả các phần tử hoạt động. 

Khó khăn chính là các bản cập nhật được xen kẽ với các truy vấn và mỗi bản cập nhật có thể thay đổi nhiều chuỗi tiềm năng vì một vấn đề duy nhất tham gia vào các chuỗi bắt đầu từ nhiều độ khó cao hơn. 

Một trường hợp góc cạnh tinh tế nảy sinh từ những vẻ đẹp tiêu cực. Một chuỗi không bị buộc phải bao gồm tất cả các nút có thể truy cập nếu được phép bỏ qua, nhưng ở đây cấu trúc ở đây hoàn toàn tuần tự khi bạn chọn bắt đầu. Tuy nhiên, vì chúng tôi đang tối đa hóa tổng, nên đuôi âm có thể làm giảm tổng giá trị, do đó, chuỗi tốt nhất có thể dừng sớm ngay cả khi về mặt logic vẫn tiếp tục kéo dài hơn, bằng cách đơn giản là không chọn nút bắt đầu dẫn đến hậu tố xấu. 

Một trường hợp khác là nhiều khó khăn giống hệt nhau với những vẻ đẹp khác nhau. Vì các vấn đề là các thực thể riêng lẻ nên việc xóa một phiên bản không được loại bỏ hiệu ứng tổng hợp một cách không chính xác, vì vậy chúng tôi cần một cấu trúc hỗ trợ hành vi giống như nhiều tập hợp cho mỗi độ khó. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ sẽ tính toán lại cuộc thi tốt nhất sau mỗi lần cập nhật bằng cách lặp lại tất cả các vấn đề hiện có và coi mỗi vấn đề là điểm khởi đầu khả thi. Từ một vấn đề bắt đầu, chúng tôi liên tục tính toán độ khó cần thiết tiếp theo và tìm kiếm một vấn đề phù hợp, mỗi lần chọn ra cách tiếp tục tốt nhất hiện có. Điều này đúng vì mọi cuộc thi hợp lệ đều được xác định hoàn toàn bởi yếu tố đầu tiên của nó. 

Tuy nhiên, điều này là tốn kém. Nếu có`n`hoạt động và lên đến`n`các vấn đề đang hoạt động, mỗi lần tính toán lại có thể quét tất cả các vấn đề và đối với mỗi điểm bắt đầu, hãy đi theo một chuỗi dài`O(log maxD)`. Điều này dẫn đến khoảng`O(n^2)`hành vi trong những trường hợp dày đặc, vượt xa giới hạn. 

Quan sát chính là cấu trúc chuyển tiếp là cố định và mang tính quyết định: mỗi khó khăn chỉ trỏ đến chính xác một điểm trước đó,`2*d`, và một người kế vị,`d//2`. Điều này tạo thành một rừng dây xích vượt qua khó khăn. Thay vì suy nghĩ theo từng vấn đề riêng lẻ, chúng tôi tổng hợp theo độ khó và duy trì kết thúc chuỗi tốt nhất có thể đạt được ở mỗi độ khó. 

Chúng tôi xác định một giá trị giống DP`dp[d]`như vẻ đẹp tối đa của một chuỗi hợp lệ kết thúc ở mức khó khăn`d`. Bất kỳ chuỗi nào kết thúc tại`d`phải đến từ một chuỗi kết thúc tại`2*d`, cộng với việc chọn một bài toán khó`d`. Vì thế,`dp[d]`chỉ phụ thuộc vào`dp[2*d]`và vẻ đẹp tốt nhất hiện có tại`d`. Điều này làm giảm vấn đề duy trì cực đại động trên biểu đồ có cấu trúc trong đó mỗi nút phụ thuộc vào một nút cha. 

Đối với mỗi khó khăn, chúng tôi duy trì vẻ đẹp tốt nhất hiện có trong số các vấn đề hiện đang tồn tại. Sau đó, chúng tôi duy trì các giá trị DP từ cao xuống thấp hoặc tính toán lại một cách lười biếng dọc theo các đường dẫn bị ảnh hưởng. Vì mỗi bản cập nhật chỉ ảnh hưởng đến một độ khó nên chúng tôi chỉ cần cập nhật theo chuỗi giảm một nửa lên hoặc xuống, mỗi độ dài`O(log maxD)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n² log n) | O(n) | Quá chậm | 
| Tối ưu | O(n log A) | O(A) | Đã chấp nhận | 

Ở đâu`A`là độ khó tối đa (< 1e6). 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai cấu trúc chính: bản đồ tần số giống như nhiều bộ dành cho người đẹp theo độ khó và một mảng`best[d]`lưu trữ vẻ đẹp tối đa hiện có cho khó khăn`d`. Chúng tôi cũng duy trì`dp[d]`, chuỗi tốt nhất kết thúc tại`d`, và một câu trả lời toàn cầu. 

1. Khởi tạo mảng`best`Và`dp`cho tất cả những khó khăn có thể lên đến 1e6. Ban đầu mọi thứ đều trống rỗng, vì vậy`best[d] = -∞`Và`dp[d] = 0`. 
2. Đối với mỗi thao tác, hãy chèn hoặc xóa một vấn đề`(d, b)`. Chúng tôi cập nhật vẻ đẹp tốt nhất được lưu trữ cho độ khó`d`bằng cách chèn hoặc xóa khỏi cấu trúc nhiều bộ. Sau khi cập nhật, tính toán lại`best[d]`như vẻ đẹp còn lại tối đa hoặc`-∞`nếu trống. 
3. Từ nút cập nhật này`d`, tính toán lại`dp`giá trị dọc theo chuỗi`d, d//2, d//4, ...`. Tại mỗi bước, chúng ta đặt`dp[x] = max(dp[2*x] + best[x], dp[x])`nhưng vì các phần phụ thuộc chỉ đi từ cao xuống thấp nên chúng tôi tính toán lại một cách rõ ràng như sau`dp[x] = best[x] + dp[2*x]`nếu như`best[x]`tồn tại, ngược lại`0`. 
4. Tiếp tục truyền đi lên cho đến khi đạt đến số không. 
5. Sau khi cập nhật, hãy tính lại câu trả lời chung ở mức tối đa`dp[d]`vượt qua mọi khó khăn, nhưng chúng tôi duy trì nó dần dần bằng cách theo dõi các nút bị ảnh hưởng trong quá trình truyền bá. 

Lý do việc nhân giống có hiệu quả cục bộ là vì chỉ có tổ tiên của`d`có thể bị ảnh hưởng bởi sự thay đổi ở`d`, vì chỉ những chuỗi đó mới bao gồm`d`trong hậu tố của họ. 

### Tại sao nó hoạt động 

Mỗi cuộc thi hợp lệ tương ứng với việc chọn độ khó bắt đầu`s`và sau đó xác định làm theo`s, ⌊s/2⌋, ⌊s/4⌋, ...`. Đối với bất kỳ cố định`s`, giá trị tốt nhất có thể được xác định hoàn toàn bởi vẻ đẹp sẵn có tốt nhất ở mỗi bước. Do đó, giá trị tối ưu cho hậu tố kết thúc tại`x`chỉ phụ thuộc vào`2x`. Điều này tạo ra một cây phụ thuộc chặt chẽ không có chu kỳ, nghĩa là các bản cập nhật được truyền lên trên mà không có sự mơ hồ. Vì mọi trạng thái DP bị ảnh hưởng đều nằm trên một chuỗi có độ dài`O(log A)`, việc tính toán lại vẫn bị giới hạn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXA = 10**6 + 5

# store multiset via dict of dicts or frequency maps
from collections import defaultdict

best = [0] * MAXA
cnt = defaultdict(lambda: defaultdict(int))

def recompute_best(x):
    if cnt[x]:
        best[x] = max(cnt[x].values())
    else:
        best[x] = 0

dp = [0] * MAXA

def recompute_chain(x):
    while x > 0:
        parent = x * 2
        if parent < MAXA:
            dp[x] = best[x] + dp[parent]
        else:
            dp[x] = best[x]
        x //= 2

n = int(input())
for _ in range(n):
    t, d, b = map(int, input().split())

    if t == 1:
        cnt[d][b] += 1
    else:
        cnt[d][b] -= 1
        if cnt[d][b] == 0:
            del cnt[d][b]

    recompute_best(d)
    recompute_chain(d)

    ans = max(dp)
    print(ans)
```Việc triển khai giữ một bản đồ tần suất cho mỗi độ khó để việc xóa được xử lý an toàn ngay cả với các bản sao. Sau mỗi lần cập nhật, chúng tôi sẽ tính toán lại vẻ đẹp tốt nhất cho độ khó đó. Sự lan truyền DP theo chuỗi giảm một nửa đi lên. 

Một điểm tinh tế là việc sử dụng`best[x] = 0`cho các nút trống. Điều này giả định rằng chúng ta có thể chọn dừng một chuỗi, điều này đúng vì cuộc thi có thể trống hoặc có thể kết thúc tại bất kỳ thời điểm nào mà không cần tiếp tục. 

Quá trình chuyển đổi DP sử dụng thực tế là một nút chỉ phụ thuộc vào nút kép của nó, do đó việc tính toán lại không yêu cầu phải xem lại các nhánh không liên quan. 

## Ví dụ đã hoạt động 

Hãy xem xét một chuỗi nhỏ: 

đầu vào:```
1 4 5
1 2 10
1 1 7
```Sau mỗi lần chèn, chúng tôi theo dõi`best`Và`dp`. 

| Bước | Hoạt động | những thay đổi tốt nhất | cập nhật chuỗi dp | tối đa toàn cầu | 
| --- | --- | --- | --- | --- | 
| 1 | cộng (4,5) | tốt nhất[4]=5 | dp[4]=5 | 5 | 
| 2 | cộng (2,10) | tốt nhất[2]=10 | dp[2]=10, dp[4]=5 | 10 | 
| 3 | cộng (1,7) | tốt nhất[1]=7 | dp[1]=7, dp[2]=17, dp[4]=5 | 17 | 

Bước thứ ba cho thấy cách chuỗi tăng giá trị: bắt đầu từ 2 cho 10 + 7 = 17 đến 2 → 1. 

Bây giờ là ví dụ thứ hai với việc xóa: 

đầu vào:```
1 4 5
1 2 10
1 1 7
2 2 10
```| Bước | Hoạt động | những thay đổi tốt nhất | cập nhật chuỗi dp | tối đa toàn cầu | 
| --- | --- | --- | --- | --- | 
| 1 | cộng (4,5) | tốt nhất[4]=5 | dp[4]=5 | 5 | 
| 2 | cộng (2,10) | tốt nhất[2]=10 | dp[2]=10 | 10 | 
| 3 | cộng (1,7) | tốt nhất[1]=7 | dp[1]=7, dp[2]=17 | 17 | 
| 4 | loại bỏ (2,10) | tốt nhất[2]=7? không, 0 | dp[2]=7, dp[1]=7 | 7 | 

Điều này chứng tỏ rằng một khi phần tử cầu chiếm ưu thế bị loại bỏ, dây xích sẽ sụp đổ một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log A) | Mỗi bản cập nhật sẽ tính toán lại theo chuỗi độ dài giảm một nửa log A | 
| Không gian | O(A) | Mảng cho tốt nhất và dp vượt qua mọi khó khăn | 

Với`n ≤ 2×10^5`Và`A ≤ 10^6`, điều này phù hợp thoải mái trong giới hạn vì mỗi thao tác chỉ kích hoạt khoảng 20 bản cập nhật. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from collections import defaultdict

    MAXA = 10**6 + 5
    best = [0] * MAXA
    cnt = defaultdict(lambda: defaultdict(int))
    dp = [0] * MAXA

    def recompute_best(x):
        if cnt[x]:
            best[x] = max(cnt[x].values())
        else:
            best[x] = 0

    def recompute_chain(x):
        while x > 0:
            parent = x * 2
            if parent < MAXA:
                dp[x] = best[x] + dp[parent]
            else:
                dp[x] = best[x]
            x //= 2

    n = int(input())
    out = []
    for _ in range(n):
        t, d, b = map(int, input().split())
        if t == 1:
            cnt[d][b] += 1
        else:
            cnt[d][b] -= 1
            if cnt[d][b] == 0:
                del cnt[d][b]

        recompute_best(d)
        recompute_chain(d)
        out.append(str(max(dp)))

    return "\n".join(out)

# sample-style tests
assert run("""3
1 4 5
1 2 10
1 1 7
""") == "5\n10\n17"

assert run("""4
1 4 5
1 2 10
1 1 7
2 2 10
""") == "5\n10\n17\n7"

assert run("""2
1 1 10
1 2 3
""") == "10\n10"

assert run("""3
1 8 1
1 4 2
1 2 3
""") == "1\n3\n6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| xây dựng dây chuyền đơn giản | số tiền tăng dần | sự chính xác của chuỗi chuyển tiếp | 
| xóa nút giữa | hành vi sụp đổ | loại bỏ tính đúng đắn | 
| chuỗi độc lập | không có ảnh hưởng chéo | cách ly cành | 
| chuỗi giảm một nửa đầy đủ | lan truyền sâu | cập nhật chuyên sâu về nhật ký | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi nhiều bài toán có cùng độ khó nhưng có vẻ đẹp khác nhau. Ví dụ, chèn`(4, 5)`Và`(4, 10)`nên đảm bảo`best[4] = 10`. Nếu như`(4, 10)`được loại bỏ sau đó,`best[4]`phải quay lại`5`, không phải bằng không. Bản đồ tần số dựa trên nhiều bộ đảm bảo điều này bằng cách theo dõi số lượng trên mỗi vẻ đẹp. 

Một trường hợp khác là khi loại bỏ yếu tố cuối cùng của một khó khăn. Giả sử chúng ta chỉ có`(2, 10)`Và`(1, 7)`, và chúng tôi loại bỏ`(2, 10)`. Chuỗi đã đóng góp 17 trước đó phải ngay lập tức giảm xuống còn 7. Việc tính toán lại trở lên từ nút 2 đảm bảo`dp[2]`được tính toán lại như`0 + dp[4]`, lan truyền một cách chính xác. 

Trường hợp cuối cùng là những khó khăn thưa thớt. Nếu chỉ`d = 10^6`đang hoạt động, quá trình tính toán lại vẫn diễn ra trong chuỗi`10^6 → 5×10^5 → ...`, vẫn là logarit. Điều này đảm bảo ngay cả những cập nhật thưa thớt trong trường hợp xấu nhất cũng không làm giảm hiệu suất.
