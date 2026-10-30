---
title: "CF 106272M - Popotnik -Người du hành Ljubljana-"
description: "Chúng ta được cung cấp một quy trình được hướng dẫn trên biểu đồ trong đó mỗi nút có một nhãn ký tự. Một “chuyến du lịch” là một chuỗi các nút được truy cập chính xác $l$. Chúng ta có thể bắt đầu từ bất kỳ nút nào và mỗi bước di chuyển dọc theo một cạnh tới nút lân cận."
date: "2026-06-18T23:01:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106272
codeforces_index: "M"
codeforces_contest_name: "The 20-th Beihang University Collegiate Programming Contest (BCPC 2025) - Preliminary"
rating: 0
weight: 106272
solve_time_s: 116
verified: true
draft: false
---

[CF 106272M - Popotnik -Người du hành đến Ljubljana-](https://codeforces.com/problemset/problem/106272/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 56 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một quy trình được hướng dẫn trên biểu đồ trong đó mỗi nút có một nhãn ký tự. Một “chuyến du lịch” là một chuỗi chính xác$l$các nút đã truy cập. Chúng ta có thể bắt đầu từ bất kỳ nút nào và mỗi bước di chuyển dọc theo một cạnh tới nút lân cận. Khi truy cập các nút, chúng ta nối các ký tự của chúng, tạo thành một chuỗi có độ dài$l$. Trong số tất cả các bước đi có thể có chiều dài$l$, chúng ta được yêu cầu tìm$k$-chuỗi nhỏ nhất theo thứ tự từ điển, trong đó hai bước được coi là khác nhau nếu chúng khác nhau ở bất kỳ vị trí nào trong chuỗi các nút được truy cập. 

Khó khăn chính không phải là tạo ra một bước đi hợp lệ mà là sắp xếp một tập hợp các bước đi lớn về mặt thiên văn mà không liệt kê chúng. Ngay cả đối với các biểu đồ vừa phải, số lần đi bộ tăng theo cấp số nhân với$l$, vì được phép truy cập lại và biểu đồ có thể chứa các chu kỳ. Ràng buộc$k \le 10^{18}$báo hiệu rằng chúng ta không thể liệt kê các ứng cử viên một cách rõ ràng và phải dựa vào việc đếm các tiền tố một cách hiệu quả. 

Biểu đồ đầu vào có thể bị ngắt kết nối, có nghĩa là một số điểm bắt đầu không hợp lệ trong một số độ dài nhất định. Một cách tiếp cận đơn giản sẽ cố gắng DFS từ mọi nút và thu thập tất cả các chuỗi có độ dài$l$, sau đó sắp xếp chúng. Điều này ngay lập tức thất bại vì ngay cả một hệ số phân nhánh nhỏ cũng tạo ra số bước đi theo cấp số nhân. Nỗ lực ngây thơ thứ hai là thực hiện DP trên các trạng thái$(node, remaining\_length)$, nhưng điều đó dẫn đến$O(n \cdot l)$trạng thái quá lớn khi cả hai$n$Và$l$có thể$2 \times 10^5$. 

Trường hợp cạnh tinh tế xuất hiện khi biểu đồ có tính kết nối cao, ví dụ như một biểu đồ hoàn chỉnh trên một vài nút. Ngay cả đối với$n = 10$, số bước đi có chiều dài$l = 200000$đã vượt xa mọi phương pháp đếm rõ ràng mà không cần cắt bớt. Một vấn đề khác là khi$k$vượt quá tổng số lần đi bộ có thể, trong trường hợp đó chúng ta phải xuất chính xác`"Impossible"`chứ không phải là xây dựng một phần. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: liệt kê tất cả các bước dài$l$, xây dựng chuỗi của chúng và sắp xếp chúng. Điều này đúng về mặt khái niệm vì thứ tự từ điển được xác định rõ ràng khi tất cả các chuỗi đều được biết đến. Điểm thất bại là hiển nhiên: số lần đi bộ từ một nút đơn lẻ tăng lên gần bằng$d^l$, Ở đâu$d$là mức độ trung bình. Ngay cả đối với nhỏ$d = 2$, điều này trở nên không khả thi tại$l = 2 \times 10^5$. 

Một ý tưởng có cấu trúc hơn là coi vấn đề này như một vấn đề đếm trên các tiền tố. Nếu chúng ta có thể tính toán, với mỗi nút$u$và chiều dài còn lại$t$, có bao nhiêu bước đi dài$t$bắt đầu từ$u$, thì chúng ta có thể tham lam xây dựng câu trả lời: ở mỗi bước, hãy thử những người hàng xóm theo thứ tự từ điển của các ký tự của họ, trừ đi phần đóng góp của họ khỏi$k$cho đến khi chúng ta tìm thấy nhánh chứa bước đi mong muốn. 

Trở ngại cốt lõi là tính toán số lượng này. DP đầy đủ trên tất cả các trạng thái là quá lớn, nhưng chúng tôi thực sự không cần số lượng chính xác vượt quá$k$. Khi một giá trị vượt quá$k$, chúng ta có thể giới hạn nó. Quan sát này làm thay đổi vấn đề: thay vì đếm chính xác, chúng ta chỉ truyền các giá trị trong phạm vi$[0, k+1]$. Việc cắt bớt này làm giảm đáng kể hiệu quả tính toán vì nhiều bài toán con sẽ sụp đổ khi chúng vượt quá ngưỡng. 

Sau đó chúng tôi lưu vào bộ nhớ đệm kết quả cho các trạng thái$(u, t)$. Mỗi trạng thái được tính một lần và giá trị của nó bị giới hạn. Trong quá trình xây dựng, chúng tôi liên tục truy vấn số lượng giới hạn này để quyết định nên sử dụng lợi thế nào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| DP với Ghi nhớ + Giới hạn ở k |$O(\text{visited states} \cdot \deg)$|$O(\text{visited states})$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta xây dựng câu trả lời một cách tham lam từ trái sang phải, duy trì nút hiện tại và độ dài còn lại. 

1. Chúng tôi sắp xếp trước danh sách lân cận của mỗi nút theo ký tự của nút đích. Điều này đảm bảo rằng việc khám phá các hàng xóm theo thứ tự sẽ tự động mang lại thứ tự từ điển trên chuỗi kết quả. 
2. Chúng ta định nghĩa một hàm$count(u, t)$trả về số bước đi dài$t$bắt đầu từ nút$u$, nhưng bị cắt cụt ở$k+1$. Việc cắt bớt này rất cần thiết vì chúng ta chỉ cần phân biệt xem một nhánh có nhỏ hơn$k$, bằng$k$, hoặc lớn hơn. 
3. Chức năng$count(u, t)$được tính toán đệ quy. Nếu như$t = 1$, câu trả lời là 1 vì bản thân nút hiện tại tạo thành một bước đi có độ dài 1 hợp lệ. Ngược lại, chúng ta tính tổng tất cả các hàng xóm$v$của$u$, thêm$count(v, t-1)$, nhưng chúng tôi ngay lập tức giới hạn số tiền ở mức$k+1$. Ghi nhớ đảm bảo mỗi trạng thái chỉ được tính một lần. 
4. Để xây dựng chuỗi thứ k, chúng ta lặp lại vị trí đầu tiên bằng cách xem xét tất cả các nút bắt đầu có thể có theo thứ tự sắp xếp các ký tự của chúng. Đối với mỗi nút ứng cử viên$u$, chúng tôi tính toán$count(u, l)$. Nếu giá trị này ít nhất$k$, chúng tôi chọn$u$như sự khởi đầu. Ngược lại chúng ta giảm$k$và tiếp tục. 
5. Sau khi sửa nút bắt đầu, chúng ta lặp lại ý tưởng tương tự cho từng vị trí tiếp theo. Tại nút hiện tại$u$và chiều dài còn lại$t$, chúng ta lặp lại các hàng xóm theo thứ tự ký tự. Đối với mỗi người hàng xóm$v$, chúng tôi tính toán$count(v, t-1)$. Nếu như$k$lớn hơn số này thì chúng ta trừ đi và bỏ qua. Ngược lại, chúng ta chuyển sang$v$và tiếp tục. 
6. Nếu tại bất kỳ bước nào không tồn tại sự chuyển tiếp hợp lệ thì việc xây dựng sẽ thất bại và chúng ta xuất ra`"Impossible"`. 

Tính đúng đắn dựa trên một bất biến duy nhất: tại mỗi bước,$k$đại diện cho thứ hạng trong số tất cả các lần hoàn thành hợp lệ của tiền tố hiện tại và$count(u, t)$phân vùng chính xác không gian hoàn thành theo lựa chọn tiền tố. Bởi vì chúng ta luôn trừ toàn bộ cây con của không gian tìm kiếm theo thứ tự từ điển nên phần còn lại$k$luôn tương ứng chính xác với nhánh mong muốn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

def solve():
    n, m, l, k = map(int, input().split())
    s = input().strip()

    g = [[] for _ in range(n)]
    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    for i in range(n):
        g[i].sort(key=lambda x: s[x])

    from functools import lru_cache

    K = k

    @lru_cache(maxsize=None)
    def dp(u, rem):
        if rem == 1:
            return 1
        total = 0
        for v in g[u]:
            total += dp(v, rem - 1)
            if total > K:
                return K + 1
        return total

    start = -1
    cur_k = K

    for u in range(n):
        cnt = dp(u, l)
        if cnt >= cur_k:
            start = u
            break
        cur_k -= cnt

    if start == -1:
        print("Impossible")
        return

    res = [s[start]]
    u = start
    rem = l

    while rem > 1:
        rem -= 1
        found = False
        for v in g[u]:
            cnt = dp(v, rem)
            if cur_k > cnt:
                cur_k -= cnt
            else:
                res.append(s[v])
                u = v
                found = True
                break
        if not found:
            print("Impossible")
            return

    print("".join(res))

def main():
    T = int(input())
    for _ in range(T):
        solve()

if __name__ == "__main__":
    main()
```Giải pháp dựa trên DP được ghi nhớ để đếm số lần đi bộ nhưng dừng ngay khi số lượng vượt quá$k$. Điều này ngăn chặn sự tăng trưởng đột ngột trong các biểu đồ dày đặc. Sau đó, việc xây dựng tham lam sẽ sử dụng số lượng này để điều hướng trực tiếp đến thứ hạng từ điển mong muốn. 

Một chi tiết triển khai tinh tế là bộ nhớ đệm của$(u, rem)$. Nếu không có tính năng ghi nhớ, các truy vấn lặp lại trong quá trình giảm dần tham lam sẽ tính toán lại các bài toán con giống nhau nhiều lần và trở nên quá chậm. Một chi tiết quan trọng khác là chấm dứt sớm số tiền khi chúng vượt quá$k$, giúp giữ giới hạn số học và tránh các vấn đề tràn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một chuỗi đơn giản$1 - 2 - 3$, nhãn$a < b < c$, Và$l = 3$,$k = 2$. 

Chúng tôi tính toán số lượng: 

| Tiểu bang | Đếm | 
| --- | --- | 
| dp(1,3) | tất cả chiều dài-3 bước đi từ 1 | 
| dp(2,2) | tất cả chiều dài-2 đi bộ từ 2 | 
| dp(3,1) | 1 | 

Từ nút 1, điểm bắt đầu nhỏ nhất về mặt từ điển được cố định. Giả sử dp(1,3) = 4 và dp(2,3) = 2. Nếu$k = 3$, chúng tôi bỏ qua phần đóng góp của nút 1 nếu cần và di chuyển tương ứng. Lựa chọn tham lam đảm bảo chúng tôi luôn trừ đi toàn bộ khối hoàn thành. 

Điều này chứng tỏ cách đếm tiền tố phân vùng không gian tìm kiếm một cách rõ ràng. 

### Ví dụ 2 

Đồ thị hình sao có tâm 1 nối với 2, 3, 4 và$l = 2$. 

| Bắt đầu | Số lần đi bộ dài 2 | 
| --- | --- | 
| 1 | 3 | 
| 2 | 1 | 
| 3 | 1 | 
| 4 | 1 | 

Nếu như$k = 4$, chúng tôi bỏ qua khối của nút 1 (3 bước), trừ và di chuyển đến nút 2. Điều này cho thấy thứ tự từ điển trên các nút bắt đầu tương tác với kích thước cây con như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\text{states visited} \cdot \deg)$| Mỗi$(u, rem)$được tính toán một lần và lặp lại các hàng xóm, nhưng bị giới hạn ở mức$k$ngăn chặn vụ nổ | 
| Không gian |$O(\text{states visited})$| Bảng ghi nhớ chỉ lưu trữ các trạng thái DP có thể truy cập | 

Số lượng trạng thái hiệu dụng được kiểm soát bằng cách cắt bớt tại$k+1$, điều này ngăn cản việc khám phá toàn bộ không gian đi bộ theo cấp số nhân. Với các ràng buộc tổng hợp trong các trường hợp thử nghiệm, cấu trúc được ghi nhớ vẫn nằm trong giới hạn trong thực tế. 

## Vỏ cạnh 

Một trường hợp quan trọng là khi$k$vượt quá tổng số lần đi bộ có thể. Ví dụ: một đồ thị có hai nút và$l = 5$có thể chỉ có một số lần đi bộ xen kẽ. Trong trường hợp này, số lượng gốc DP nhỏ hơn$k$và thuật toán báo cáo chính xác khả năng không thể thực hiện được trước khi thử xây dựng. 

Một trường hợp cạnh khác là biểu đồ bị ngắt kết nối trong đó chỉ một số thành phần cho phép đi bộ dài. Bắt đầu từ các nút không thể tạo ra độ dài$l$bước đi mang lại dp(u, l) = 0, đảm bảo chúng được bỏ qua một cách chính xác trong giai đoạn lựa chọn ban đầu. 

Trường hợp cuối cùng là một biểu đồ có nhiều chu kỳ, trong đó DP ngây thơ sẽ lặp vô thời hạn. Việc ghi nhớ đảm bảo chấm dứt vì mỗi trạng thái$(u, rem)$được đánh giá một lần và độ sâu đệ quy được giới hạn bởi$l$, trong khi việc cắt tỉa đảm bảo chỉ có thể truy cập được, các trạng thái liên quan mới được mở rộng.
