---
title: "CF 104491A - Vấn đề dễ dàng"
description: "Chúng ta được cấp một hàng gà được đánh số từ trái sang phải. Mỗi con gà đều có giới hạn sức chứa, nghĩa là nó chỉ được ăn tối đa một số lượng ngũ cốc nhất định."
date: "2026-06-30T12:27:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104491
codeforces_index: "A"
codeforces_contest_name: "43rd Petrozavodsk Programming Camp (2022 Summer) Day 7. HSE Koresha Contest"
rating: 0
weight: 104491
solve_time_s: 88
verified: false
draft: false
---

[CF 104491A - Vấn đề dễ](https://codeforces.com/problemset/problem/104491/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một hàng gà được đánh số từ trái sang phải. Mỗi con gà đều có giới hạn sức chứa, nghĩa là nó chỉ được ăn tối đa một số lượng ngũ cốc nhất định. Bên cạnh đó, còn có một số máng ăn và mỗi máng ăn bao gồm một phân khúc gà liên tục và chứa một số lượng ngũ cốc cố định. 

Điều khó khăn nằm ở cách lọc thức ăn cho từng con gà. Đối với chỉ số gà cố định$i$, chúng tôi chỉ giữ lại các nguồn cấp dữ liệu có phân khúc bao gồm$i$. Tất cả các nguồn cấp dữ liệu khác đều bị bỏ qua hoàn toàn. Trong số các máng ăn còn lại, chúng tôi xem xét tổng số ngũ cốc có thể sử dụng nhưng mỗi con gà vẫn không được vượt quá khả năng của mình. 

Vì vậy với mỗi con gà$i$, chúng ta tưởng tượng một thế giới nơi chỉ có những người cho ăn che phủ$i$hiện hữu. Trong thế giới đó, chúng tôi muốn tổng số ngũ cốc được đóng góp bởi những người cho ăn đó, nhưng bị giới hạn bởi hạn chế về năng lực của mỗi con gà. 

Đầu ra là một danh sách$n$các giá trị, một giá trị cho mỗi con gà, mô tả tổng số ngũ cốc có thể sử dụng được cho con gà đó theo quy tắc lọc này. 

Các ràng buộc đủ chặt chẽ nên cách tiếp cận bậc hai đối với gà và người cho ăn sẽ quá chậm. Với$n, m \le 10^5$cho mỗi trường hợp thử nghiệm và tổng số tiền được giới hạn trong các thử nghiệm, mọi giải pháp đều phải gần tuyến tính hoặc$O((n + m)\log n)$. Việc quét từng con gà một cách đơn giản trên tất cả các máng ăn sẽ có hiệu suất lên đến$10^{10}$hoạt động trong trường hợp xấu nhất là không khả thi. 

Một điểm tinh tế phát sinh từ hành vi chồng chéo. Một máng ăn góp phần tạo ra nhiều con gà, nhưng đối với mỗi con gà, chúng tôi quyết định xem nó có được đưa vào hay không một cách độc lập. Điều này có nghĩa là sự đóng góp của một nhánh trung chuyển là “toàn cầu cục bộ”: nó chỉ phụ thuộc vào việc chỉ số có nằm trong phân khúc của nó hay không. 

Một sai lầm ngây thơ là cho rằng chúng ta phải tính lại số tiền từ đầu cho mỗi con gà. Một sai lầm khác là quên rằng một nguồn cấp dữ liệu bị loại trừ cho một chỉ mục vẫn có thể được đưa vào một chỉ mục gần đó, vì vậy chúng ta không thể xóa trước các nguồn cấp dữ liệu trên toàn cầu. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu sửa chữa một con gà$i$, sau đó lặp lại tất cả các nguồn cấp dữ liệu và tổng$c_j$dành cho những người có khoảng thời gian$[l_j, r_j]$chứa$i$. Điều này đúng vì nó trực tiếp tuân theo quy tắc. Tuy nhiên, mỗi truy vấn có giá$O(m)$, và làm điều này cho tất cả$n$gà dẫn đến$O(nm)$, đạt tới$10^{10}$hoạt động trong trường hợp xấu nhất. 

Quan sát quan trọng là đối với một con gà cố định$i$, một bộ cấp nguồn đóng góp khi và chỉ khi$l_j \le i \le r_j$. Chúng ta có thể viết lại thành: tất cả các nguồn cấp dữ liệu hoạt động tại$i$chính xác là những người có khoảng thời gian bao gồm$i$. Vì vậy, mỗi bộ cấp nguồn đóng góp trọng số của nó vào một loạt các chỉ số liên tục, cụ thể là từ$l_j$ĐẾN$r_j$. Đây là cấu trúc “thêm phạm vi, truy vấn điểm” cổ điển. 

Thay vì tính toán lại cho mỗi$i$, chúng ta đảo ngược quan điểm. Mỗi bộ nạp thêm$c_j$tới mọi chỉ số trong khoảng của nó. Vì vậy, chúng tôi muốn tính toán, cho mọi vị trí$i$, tổng của tất cả$c_j$trên các máng ăn bao phủ nó. Điều này trở thành một vấn đề tích lũy mảng hoặc tổng tiền tố. 

Khi chúng tôi tính toán mảng cơ sở này của tổng số tiền bảo hiểm, chúng tôi phải tôn trọng năng lực của gà. Mỗi con gà không thể ăn nhiều hơn$a_i$, vì vậy câu trả lời cuối cùng chỉ đơn giản là$\min(a_i, \text{coverage}[i])$. 

Điều này chuyển đổi vấn đề thành hai lần tuyến tính trên các mảng khác nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(1) | Quá chậm | 
| Tối ưu (mảng sai phân) | O(n + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán số lượng hạt có sẵn ở mỗi vị trí nếu chúng tôi áp dụng tất cả các bộ cấp liệu dưới dạng bổ sung phạm vi, sau đó kẹp theo công suất. 

1. Khởi tạo một mảng`diff`kích thước$n+2$với số không. Mảng này sẽ lưu trữ các cập nhật phạm vi ở dạng nén. Ý tưởng là thay vì cập nhật mọi vị trí trong một phạm vi, chúng tôi đánh dấu nơi bắt đầu và kết thúc đóng góp. 
2. Đối với mỗi máng ăn$(l_j, r_j, c_j)$, thêm vào$c_j$Tại`diff[l_j]`và trừ$c_j$Tại`diff[r_j + 1]`. Điều này mã hóa rằng sự đóng góp chỉ hoạt động giữa$l_j$Và$r_j$. Phép trừ đảm bảo hiệu ứng dừng lại sau khi phạm vi kết thúc. 
3. Xây dựng tổng tiền tố`diff`để khôi phục mảng phủ sóng thực tế`cover[i]`. Tại mỗi chỉ số$i$,`cover[i]`bằng tổng của tất cả các đóng góp của bộ nạp đang hoạt động tại thời điểm đó. Điều này hoạt động vì mọi cập nhật phạm vi đều đóng góp +c khi bắt đầu và -c sau khi kết thúc, do đó, việc tích lũy tiền tố sẽ tái tạo lại tổng chồng chéo chính xác. 
4. Cho mỗi con gà$i$, tính toán câu trả lời cuối cùng là$\min(a_i, cover[i])$. Điều này thực thi giới hạn năng lực của mỗi con gà. 
5. Xuất ra tất cả các câu trả lời cho test case. 

### Tại sao nó hoạt động 

Mỗi nguồn cấp dữ liệu đóng góp$c_j$chính xác các chỉ số trong khoảng của nó. Mảng chênh lệch đảm bảo rằng mỗi khoảng như vậy được tính chính xác một lần trong quá trình tính tổng tiền tố. Không có đóng góp nào bị mất hoặc bị tính hai lần vì mỗi lần bổ sung đều bị hủy tương ứng sau khi khoảng thời gian kết thúc. Tổng tiền tố kết quả tại vị trí$i$bằng tổng của tất cả các nguồn cấp dữ liệu bao gồm$i$, chính xác là số lượng cần thiết trước khi áp dụng giới hạn công suất. Vì công suất không phụ thuộc vào mỗi chỉ số nên việc kẹp không ảnh hưởng đến tính chính xác của các vị trí khác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))

        diff = [0] * (n + 2)

        for _ in range(m):
            l, r, c = map(int, input().split())
            diff[l] += c
            diff[r + 1] -= c

        cur = 0
        res = [0] * n

        for i in range(1, n + 1):
            cur += diff[i]
            res[i - 1] = min(a[i - 1], cur)

        print(*res)

if __name__ == "__main__":
    solve()
```Việc triển khai phản ánh chính xác thuật toán. các`diff`mảng có kích thước$n+2$để xử lý một cách an toàn$r+1$cập nhật mà không cần kiểm tra giới hạn. Biến tích lũy tiền tố`cur`tránh tính toán lại các tổng tiền tố nhiều lần. Mỗi vị trí được xử lý một lần, đảm bảo hiệu suất tuyến tính cho mỗi trường hợp thử nghiệm. 

Một sai lầm phổ biến là quên rằng việc lập chỉ mục dựa trên 1 trong đầu vào nhưng dựa trên 0 trong mảng Python. Giải pháp này chỉ thay đổi liên tục khi viết vào`res`, giữ`diff`căn chỉnh với logic dựa trên 1 để rõ ràng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một mô hình nhỏ với ba con gà và hai máng ăn. 

Giải thích đầu vào: 

Máng 1 che gà từ 1 đến 2 bằng 3 hạt, máng 2 che gà từ 2 đến 3 bằng 2 hạt. Công suất là$a = [2, 5, 2]$. 

Chúng tôi xây dựng mảng khác biệt từng bước. 

| Bước | Hoạt động | mảng khác biệt (1..3) | cur | che[i] | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | bắt đầu | [0,0,0,0] | 0 | - | - | 
| F1 | +3 lúc 1, -3 lúc 3 | [3,0,-3,0] | - | - | - | 
| F2 | +2 lúc 2, -2 lúc 4 | [3,2,-3,-2] | - | - | - | 
| tôi=1 | tiền tố | - | 3 | 3 | phút(2,3)=2 | 
| tôi=2 | tiền tố | - | 5 | 5 | phút(5,5)=5 | 
| tôi=3 | tiền tố | - | 2 | 2 | phút(2,2)=2 | 

Điều này cho thấy cách các bộ cấp dữ liệu chồng chéo tích lũy một cách tự nhiên thông qua các tổng tiền tố và dung lượng chỉ bị hạn chế sau khi tổng hợp đầy đủ. 

### Ví dụ 2 

Trường hợp có các bộ cấp dữ liệu không chồng chéo làm nổi bật tính độc lập. 

Giả định$n=4$, năng lực$a=[1,10,1,10]$, người ăn:$[1,1,5]$,$[3,4,7]$. 

| tôi | bảo hiểm cur | công suất | kết quả | 
| --- | --- | --- | --- | 
| 1 | 5 | 1 | 1 | 
| 2 | 0 | 10 | 0 | 
| 3 | 7 | 1 | 1 | 
| 4 | 7 | 10 | 7 | 

Mỗi phân đoạn chỉ ảnh hưởng đến khoảng thời gian riêng của nó và các vị trí bên ngoài nhận được 0 do không có bộ cấp nguồn hoạt động. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) cho mỗi trường hợp thử nghiệm | Mỗi bộ cấp dữ liệu được xử lý một lần và mỗi vị trí được truy cập một lần trong quá trình tính toán tiền tố | 
| Không gian | O(n) | Mảng sai phân và mảng kết quả có kích thước tỉ lệ với n | 

Các ràng buộc tổng thể đảm bảo rằng tổng của tất cả$n$Và$m$trên nhiều trường hợp thử nghiệm là nhiều nhất$10^5$, vì vậy cách tiếp cận tuyến tính này nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out_lines = []
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        diff = [0] * (n + 2)

        for _ in range(m):
            l, r, c = map(int, input().split())
            diff[l] += c
            diff[r + 1] -= c

        cur = 0
        res = []
        for i in range(1, n + 1):
            cur += diff[i]
            res.append(str(min(a[i - 1], cur)))

        out_lines.append(" ".join(res))

    return "\n".join(out_lines)

# sample-like tests
assert run("""1
3 2
2 5 2
1 2 3
2 3 2
""") == "2 5 2"

# minimum size
assert run("""1
1 1
10
1 1 5
""") == "5"

# no feeders
assert run("""1
3 0
1 2 3
""") == "0 0 0"

# full overlap
assert run("""1
4 2
1 1 1 1
1 4 10
2 3 5
""") == "1 1 1 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chồng chéo nhỏ duy nhất | 2 5 2 | tính đúng đắn của sự tích lũy chồng chéo | 
| n=1 trường hợp | 5 | xử lý ranh giới cho r+1 | 
| không có người cho ăn | 0 0 0 | xử lý cập nhật trống | 
| trộn chồng chéo đầy đủ | 1 1 1 1 | sự thống trị kẹp công suất | 

## Vỏ cạnh 

Trường hợp một con gà tối thiểu có một bộ nạp duy nhất sẽ kiểm tra xem phép trừ r+1 có phá vỡ giới hạn mảng hay không. Đối với đầu vào$n=1$,$l=r=1$, bản cập nhật ghi vào`diff[2]`, điều này hợp lệ vì mảng có kích thước$n+2$. Tổng tiền tố tạo ra phạm vi bao phủ chính xác và việc kẹp đảm bảo giá trị cuối cùng bằng$\min(a_1, c_1)$. 

Trường hợp không có bộ cấp dữ liệu đảm bảo tổng tiền tố vẫn bằng 0 ở mọi nơi. Thuật toán xuất ra các số 0 một cách chính xác bất kể dung lượng là bao nhiêu, vì không có bản cập nhật phạm vi nào được kích hoạt. 

Một tập hợp đầy đủ các bộ cấp dữ liệu chồng chéo sẽ kiểm tra tính chính xác của việc tích lũy. Mọi chỉ mục đều nhận được tổng của tất cả các giá trị cấp dữ liệu và thao tác tối thiểu cuối cùng đảm bảo rằng ngay cả các phần chồng chéo rất lớn cũng được giới hạn độc lập trên mỗi vị trí, xác nhận rằng tính độc lập trên mỗi chỉ mục được duy trì.
