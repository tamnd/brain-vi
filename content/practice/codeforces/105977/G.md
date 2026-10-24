---
title: "CF 105977G - \u7092\u80a1\u9ad8\u624b"
description: "Bài toán đưa ra một chuỗi giá cổ phiếu trong $n$ ngày, nhưng giá được mã hóa dưới dạng logarit. Vào ngày $i$, giá thực tế là $e^{ai}$, trong đó $ai$ là số nguyên dương."
date: "2026-06-22T16:28:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105977
codeforces_index: "G"
codeforces_contest_name: "2025 National Invitational of CCPC (Fujian), The 12th Fujian Collegiate Programming Contest"
rating: 0
weight: 105977
solve_time_s: 58
verified: true
draft: false
---

[CF 105977G - \u7092\u80a1\u9ad8\u624b](https://codeforces.com/problemset/problem/105977/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán đưa ra một chuỗi giá cổ phiếu trên$n$ngày, nhưng giá được mã hóa ở dạng logarit. Vào ngày$i$, giá thực tế là$e^{a_i}$, Ở đâu$a_i$là số nguyên dương. Điều này biến các khoản lãi và lỗ theo cấp số nhân trên thị trường thực thành các chênh lệch cộng gộp trong không gian số mũ. 

Mỗi truy vấn mô tả một khoảng thời gian$[s, t]$. Vào ngày$s$, chúng tôi nhận được một khoản vay$e^k$các đơn vị tiền. Chúng tôi được phép tự do giao dịch cổ phiếu trong ngày$s$bởi vì$t$, mua cổ phiếu lẻ và thực hiện giao dịch không giới hạn. Vào cuối ngày$t$, tất cả cổ phần được thanh lý và khoản vay được hoàn trả. Chúng ta muốn giá trị tài sản cuối cùng tối đa có thể có sau khi trả nợ, được thể hiện lại dưới dạng hàm mũ$e^w$, và nhiệm vụ là xuất ra$w$. 

Vì vậy, nhiệm vụ cốt lõi cho mỗi truy vấn là: bắt đầu bằng tiền mặt$e^k$, tối đa hóa số tiền cuối cùng sau khi giao dịch chênh lệch giá theo một chuỗi giá đã biết, sau đó xuất số mũ kết quả. 

Sự chuyển đổi quan trọng là vì mọi thứ đều theo cấp số nhân, nên làm việc trong không gian logarit sẽ biến lợi ích cấp số nhân thành lợi ích cộng gộp. Giao dịch từ ngày$i$Hôm nay$j$đưa ra một hệ số tăng$e^{a_j - a_i}$, vì vậy trong không gian số mũ nó đóng góp$a_j - a_i$. 

Các ràng buộc rất lớn: lên tới$10^5$ngày và$10^5$truy vấn. Điều đó ngay lập tức loại trừ bất kỳ giải pháp nào tính toán lại giao dịch tối ưu một cách độc lập cho mỗi truy vấn theo thời gian tuyến tính, vì điều đó sẽ dẫn đến$10^{10}$hoạt động. 

Một cách tiếp cận đơn giản sẽ xử lý từng truy vấn một cách độc lập và mô phỏng giao dịch tối ưu bên trong$[s, t]$. Tuy nhiên, ngay cả việc tìm ra chiến lược tối ưu cho một khoảng thời gian cũng đòi hỏi phải quét phân đoạn đó nhiều lần nếu được thực hiện cẩn thận và việc thực hiện điều đó đối với tất cả các truy vấn là không khả thi. 

Một dạng thất bại tinh tế hơn xuất phát từ việc nghĩ rằng câu trả lời chỉ phụ thuộc vào mức giá tối thiểu và tối đa trong khoảng thời gian đó. Điều đó không chính xác vì nhiều giao dịch xen kẽ sẽ làm tăng vấn đề: chiến lược tối ưu không nhất thiết phải là một lần mua và một lần bán, mà là một chuỗi các động thái có lợi. 

## Phương pháp tiếp cận 

Bước đầu tiên là hiểu “giao dịch tối ưu với giao dịch không giới hạn và chia sẻ theo tỷ lệ” nghĩa là gì trong không gian nhật ký. Giả sử chúng ta mua vào ngày$i$và bán trong ngày$j > i$. Mức tăng theo số mũ là$a_j - a_i$. Nếu chúng tôi thực hiện nhiều giao dịch, giả sử chúng tôi chia thành các phân đoạn$i_1 < i_2 < \dots < i_k$, tổng mức tăng trở thành$$(a_{i_2} - a_{i_1}) + (a_{i_3} - a_{i_2}) + \dots + (a_{i_k} - a_{i_{k-1}}),$$kính viễn vọng nào$a_{i_k} - a_{i_1}$nếu chúng ta buộc phải mua/bán xen kẽ nhau. Nhưng hạn chế thực sự là chúng ta không bị buộc phải luân phiên nghiêm ngặt; chúng ta chỉ có thể đạt được khi chuyển từ mức giá thấp hơn sang mức giá cao hơn qua các ranh giới đã chọn. 

Sự đơn giản hóa cổ điển xuất hiện khi chúng ta giải thích giao dịch là tích lũy tất cả những khác biệt tích cực tăng lên giữa các phân đoạn tăng liên tiếp. Nếu trình tự tăng lên, chúng tôi sẽ nắm bắt được mức tăng đó; nếu nó giảm, chúng tôi bỏ qua nó và chờ đợi một điểm vào lệnh tốt hơn. Điều này làm giảm mức tăng tối ưu$[s, t]$với tổng của tất cả các sai phân dương liền kề:$$\sum_{i=s}^{t-1} \max(0, a_{i+1} - a_i).$$Thì câu trả lời cuối cùng chỉ đơn giản là:$$w = k + \sum_{i=s}^{t-1} \max(0, a_{i+1} - a_i).$$Cách tiếp cận bạo lực đánh giá tổng này cho từng truy vấn một cách độc lập, quét khoảng thời gian. Đó là$O(n)$cho mỗi truy vấn, sẽ trở thành$O(nm)$tổng cộng và không thể vượt qua. 

Quan sát quan trọng là biểu thức chỉ phụ thuộc vào sai phân liền kề. Định nghĩa$d_i = \max(0, a_{i+1} - a_i)$. Sau đó, mỗi truy vấn giảm xuống thành một truy vấn tổng phạm vi trên mảng$d$. Với tổng tiền tố, chúng ta có thể trả lời từng truy vấn trong thời gian không đổi sau khi xử lý trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nm)$|$O(1)$| Quá chậm | 
| Tiền tố Tổng về sự khác biệt |$O(n + m)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi chuỗi giá thành một mảng đóng góp chênh lệch và sau đó trả lời các truy vấn về phạm vi trên đó. 

1. Xây dựng mảng phụ trợ$d$chiều dài$n-1$, trong đó mỗi phần tử là$d_i = \max(0, a_{i+1} - a_i)$. Điều này chỉ tách biệt những động thái đi lên có lợi nhuận và loại bỏ những động thái đi xuống, vì thua lỗ không góp phần vào giao dịch tối ưu. 
2. Xây dựng mảng tổng tiền tố$p$, Ở đâu$p[i]$lưu trữ tổng của$d_1$bởi vì$d_i$. Điều này cho phép chúng ta tính bất kỳ tổng khoảng nào trong thời gian không đổi. 
3. Đối với mỗi truy vấn$[s, t]$, tính tổng mức tăng trong khoảng như$p[t-1] - p[s-1]$, bởi vì chỉ những chuyển đổi hoàn toàn bên trong khoảng thời gian mới quan trọng. 
4. Cộng số mũ cơ số$k$để đạt được điều này, tạo ra số mũ cuối cùng$w$. 
5. Đầu ra$w$. 

Lý do mảng chênh lệch là đủ là vì mọi chiến lược giao dịch có lợi nhuận đều có thể được sắp xếp lại sao cho tất cả lợi nhuận đều tương ứng chính xác với các bước đi lên mà không làm thay đổi kết quả ròng. Bất kỳ chuỗi giao dịch nào cũng sẽ thu về mức tăng cục bộ. 

### Tại sao nó hoạt động 

Điều bất biến là tại bất kỳ thời điểm nào trong khoảng thời gian, chiến lược tốt nhất có thể duy trì thuộc tính là chúng ta đang nắm giữ tiền mặt hoặc nắm giữ cổ phiếu được mua ở mức tối thiểu cục bộ gần đây nhất. Mỗi khi trình tự tăng lên, mức tăng đó có thể được nắm bắt đầy đủ mà không ảnh hưởng đến các quyết định trong tương lai. Mọi sự giảm giá đều bị bỏ qua vì việc vào đó không thể cải thiện việc thoát ra sau này so với việc chờ đợi mức giá thấp hơn. 

Điều này làm giảm tất cả các chiến lược tối ưu thành sự đóng góp độc lập của các phân đoạn đơn điệu cục bộ, làm cho tổng các khác biệt dương liền kề vừa cần thiết vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
a = list(map(int, input().split()))
k = int(input())

n = len(a)
d = [0] * (n - 1)

for i in range(n - 1):
    if a[i + 1] > a[i]:
        d[i] = a[i + 1] - a[i]

prefix = [0] * (n)
for i in range(n - 1):
    prefix[i + 1] = prefix[i] + d[i]

out = []
for _ in range(m):
    s, t = map(int, input().split())
    gain = prefix[t - 1] - prefix[s - 1]
    out.append(str(k + gain))

print("\n".join(out))
```Mã đầu tiên xây dựng mảng khuếch đại bằng cách quét các sai phân liền kề một lần. Mảng tiền tố được dịch chuyển theo một chỉ mục để các truy vấn phạm vi căn chỉnh rõ ràng với đầu vào có 1 chỉ mục. 

Mỗi truy vấn chuyển đổi trực tiếp thành một sự khác biệt tiền tố. Việc bổ sung$k$được thực hiện tại thời điểm truy vấn để tránh lặp lại số học trong quá trình tiền xử lý. 

Một cạm bẫy phổ biến là lập chỉ mục từng cái một: mảng khuếch đại có kích thước$n-1$, nhưng truy vấn đề cập đến ngày. Bản đồ là các cơ hội giao dịch nằm giữa ngày$i$Và$i+1$, vậy truy vấn$[s, t]$bao gồm các chỉ số$[s, t-1]$trong mảng khác biệt. 

## Ví dụ đã hoạt động 

Hãy xem xét hành vi mẫu được mô tả trong tuyên bố. 

Đối với khoảng thời gian đầu tiên$[2, 4]$với giá cả$[2, 4, 5]$, sự khác biệt là$+2, +1$. Tổng tiền tố mang lại mức tăng tích lũy$3$. Bắt đầu từ$k = 2$, số mũ cuối cùng trở thành$5$. 

| Bước | Khoảng thời gian | Tổng chênh lệch | Kết quả tiền tố | Cuối cùng w | 
| --- | --- | --- | --- | --- | 
| 1 | [2,4] | 2 + 1 = 3 | 3 | 5 | 

Điều này cho thấy rằng nhiều lần tăng liên tiếp được tích lũy thay vì được coi là một hoạt động mua-bán duy nhất. 

Đối với khoảng thứ hai$[3, 6]$với giá cả$[4, 5, 3, 6]$, sự khác biệt là$+1, 0, +3$, tổng hợp thành$4$. Thêm cơ sở$k=2$sản lượng$6$. 

| Bước | Khoảng thời gian | Tổng chênh lệch | Kết quả tiền tố | Cuối cùng w | 
| --- | --- | --- | --- | --- | 
| 1 | [3,6] | 1 + 0 + 3 = 4 | 4 | 6 | 

Điều này chứng tỏ rằng thuật toán bỏ qua các chuyển đổi đi xuống một cách chính xác và chỉ tích lũy các phân khúc có lợi nhuận. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + m)$| Một lần vượt qua sẽ tạo ra sự khác biệt và tổng tiền tố, mỗi truy vấn được trả lời trong O(1) | 
| Không gian |$O(n)$| Lưu trữ mảng khác biệt và tiền tố | 

Giải pháp phù hợp thoải mái trong các ràng buộc vì cả hai$n$Và$m$là$10^5$và tất cả các hoạt động là tuyến tính hoặc thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    k = int(input())

    n = len(a)
    d = [0] * (n - 1)
    for i in range(n - 1):
        if a[i + 1] > a[i]:
            d[i] = a[i + 1] - a[i]

    prefix = [0] * (n)
    for i in range(n - 1):
        prefix[i + 1] = prefix[i] + d[i]

    out = []
    for _ in range(m):
        s, t = map(int, input().split())
        out.append(str(k + prefix[t - 1] - prefix[s - 1]))

    return "\n".join(out)

# minimal size
assert run("1 1\n5\n3\n1 1") == "3"

# strictly decreasing
assert run("5 1\n5 4 3 2 1\n10\n1 5") == "10"

# strictly increasing
assert run("5 1\n1 2 3 4 5\n10\n1 5") == "14"

# alternating
assert run("4 1\n1 3 2 4\n0\n1 4") == "4"

# sample-style
assert run("6 2\n3 2 4 5 3 6\n2\n2 4\n3 6") == "5\n6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 3 | đường cơ sở truy vấn một ngày | 
| dãy giảm dần | 10 | không có giao dịch có lợi nhuận | 
| trình tự tăng dần | 14 | tích lũy đầy đủ lợi nhuận | 
| xen kẽ | 4 | bỏ qua giọt một cách chính xác | 
| kiểu mẫu | 5,6 | tính chính xác của nhiều truy vấn | 

## Vỏ cạnh 

Một trường hợp tinh tế là khi khoảng thời gian bắt đầu hoặc kết thúc bên trong một đoạn tăng lên. Ví dụ: theo trình tự như$1, 5, 10, 2, 3$, một truy vấn$[2, 4]$chỉ bao gồm$5, 10, 2$. Thuật toán tính toán sự đóng góp chênh lệch$+5$bên trong khoảng và bỏ qua việc thả từ$10$ĐẾN$2$, vì nó không mang lại lợi nhuận. Phép trừ tiền tố chỉ cách ly chính xác ranh giới bên trong$5 \to 10$. 

Một trường hợp khác là truy vấn một ngày$[i, i]$. Không có chuyển tiếp, vì vậy tổng chênh lệch bằng 0 và câu trả lời là chính xác$k$. Công thức tiền tố đương nhiên mang lại kết quả bằng 0 vì$p[i-1] - p[i-1] = 0$, ngăn ngừa bất kỳ tác động ngoài giới hạn hoặc tác động lập chỉ mục tiêu cực nào khi được triển khai cẩn thận.
