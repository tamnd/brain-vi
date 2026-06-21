---
title: "CF 1055B - Alice và thợ làm tóc"
description: "Chúng tôi đang duy trì một loạt các độ dài tóc trên một hàng vị trí và hệ thống sẽ phát triển theo thời gian khi một số vị trí phát triển."
date: "2026-06-15T10:06:35+07:00"
tags: ["codeforces", "competitive-programming", "dsu", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1055
codeforces_index: "B"
codeforces_contest_name: "Mail.Ru Cup 2018 Round 2"
rating: 1300
weight: 1055
solve_time_s: 224
verified: true
draft: false
---

[CF 1055B - Alice và thợ làm tóc](https://codeforces.com/problemset/problem/1055/B) 

**Đánh giá:** 1300 
**Tags:** dsu, triển khai 
**Thời gian giải:** 3 phút 44s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một loạt các độ dài tóc trên một hàng vị trí và hệ thống sẽ phát triển theo thời gian khi một số vị trí phát triển. Bất cứ lúc nào chúng ta cũng có thể tưởng tượng ra một hoạt động cắt tóc giả định: mọi tư thế có tóc dài hơn một ngưỡng cố định$l$phải được giảm xuống chính xác$l$, nhưng dụng cụ cắt tóc có một hạn chế. Một thao tác chỉ có thể hoạt động trên một phân đoạn liền kề và nó chỉ hoạt động nếu mọi vị trí trong phân đoạn đó hiện vượt quá$l$. Mục tiêu của thợ cắt tóc là giảm thiểu số lượng các thao tác phân đoạn cần thiết để đưa tất cả các giá trị xuống mức tối đa$l$. Mỗi truy vấn loại 0 yêu cầu số lượng thao tác tối thiểu này ở trạng thái hiện tại, trong khi truy vấn loại một tăng vĩnh viễn một vị trí. 

Đối tượng chính không phải là các giá trị chính xác mà là mẫu của các chỉ số hiện vượt quá$l$. Bất kỳ chỉ số nào bằng hoặc thấp hơn$l$không cần thiết để cắt vì không cần chạm vào và cũng có thể làm gãy các đoạn. Điều quan trọng là có bao nhiêu khối chỉ số liền kề thỏa mãn$a_i > l$, bởi vì mỗi khối như vậy có thể được loại bỏ trong một thao tác. 

Các ràng buộc thúc đẩy giải pháp trực tuyến có độ phức tạp gần như tuyến tính. Với tối đa$10^5$cập nhật và truy vấn, việc tính toán lại câu trả lời từ đầu cho mọi truy vấn loại 0 sẽ yêu cầu quét toàn bộ mảng mỗi lần, dẫn đến$O(nm)$, quá chậm. 

Một vấn đề tế nhị phát sinh từ thực tế là các bản cập nhật chỉ làm tăng giá trị. Sự đơn điệu này là rất quan trọng. Một vị trí đã ở trên$l$không bao giờ thay đổi trạng thái của nó nữa và vị trí thấp hơn hoặc bằng$l$có thể chuyển đúng một lần, từ không hoạt động sang hoạt động, nhưng không bao giờ quay lại. 

Một sai lầm đơn giản là tính toán lại số lượng phân đoạn bằng cách chỉ quét vùng đã thay đổi hoặc cố gắng duy trì điểm cuối của các phân đoạn chung mà không xử lý hợp nhất đúng cách. Ví dụ: hãy xem xét một cấu hình như$[1, 5, 1, 5]$với$l = 3$. Câu trả lời đúng là hai đoạn. Nếu chúng ta giả định không chính xác rằng mỗi vị trí hoạt động đóng góp độc lập một thao tác, thì chúng ta sẽ đưa ra bốn thao tác, bỏ qua hoàn toàn vị trí kề. 

Một dạng lỗi khác xuất phát từ việc quên rằng việc kích hoạt một vị trí có thể hợp nhất hai phân đoạn riêng biệt trước đó. Ví dụ: nếu chúng ta có vị trí hoạt động tại các chỉ số$1$Và$3$, câu trả lời là hai phân đoạn. Nếu chỉ mục$2$trở nên hoạt động, câu trả lời đúng sẽ trở thành một, nhưng cách tiếp cận cập nhật cục bộ mà chỉ các bộ đếm tăng dần mới bỏ lỡ mức giảm này. 

## Phương pháp tiếp cận 

Chiến lược vũ phu rất đơn giản. Đối với mỗi truy vấn loại 0, chúng tôi quét toàn bộ mảng và xây dựng một mảng boolean cho biết liệu mỗi vị trí có vượt quá hay không$l$. Sau đó, chúng tôi đếm số lần giá trị đúng xuất hiện ngay sau giá trị sai, cộng thêm một lần nếu phân đoạn hoạt động đầu tiên tồn tại. Điều này tính toán chính xác số khối liền kề, nhưng mỗi truy vấn có giá$O(n)$, làm cho độ phức tạp tổng thể$O(nm)$, quá chậm đối với trường hợp xấu nhất. 

Quan sát quan trọng là trạng thái của mỗi vị trí là nhị phân với quá trình chuyển đổi một lần. Khi một giá trị vượt qua$l$, nó vẫn ở trên$l$mãi mãi. Điều này có nghĩa là chúng ta không xử lý các khoảng động tùy ý mà xử lý các điểm kích hoạt tăng dần. Cấu trúc mà chúng tôi duy trì là số lượng thành phần được kết nối trong một tập hợp động các chỉ mục hoạt động trên một dòng. Mỗi lần kích hoạt sẽ tạo một thành phần mới hoặc hợp nhất hai thành phần hiện có. 

Điều này làm giảm vấn đề duy trì số lượng thành phần được kết nối khi chèn điểm trên biểu đồ đường. Một cấu trúc hợp tập rời rạc hoặc kiểm tra lân cận đơn giản là đủ, vì kề là một chiều. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nm)$|$O(n)$| Quá chậm | 
| Theo dõi thành phần gia tăng |$O(n + m)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải thích mọi chỉ mục là không hoạt động (giá trị$\le l$) hoặc hoạt động (giá trị$> l$). Chúng tôi duy trì số lượng khối hoạt động liền kề tồn tại. 

1. Khởi tạo một mảng`active`Ở đâu`active[i] = 1`nếu như$a_i > l$, ngược lại là 0. 

Điều này thể hiện cấu trúc ban đầu của các phân đoạn. 
2. Duyệt mảng một lần và đếm xem có bao nhiêu chỉ mục bắt đầu một phân đoạn hoạt động mới. 

Một vị trí bắt đầu một phân đoạn nếu nó đang hoạt động và đó là chỉ mục đầu tiên hoặc chỉ mục trước đó không hoạt động. 

Điều này đưa ra số lượng phân đoạn ban đầu. 
3. Duy trì một biến`segments`lưu trữ số lượng khối hoạt động hiện tại. 
4. Đối với mỗi truy vấn cập nhật làm tăng vị trí$p$, trước tiên hãy kiểm tra xem vị trí này đã hoạt động chưa. Nếu đúng như vậy thì không có gì thay đổi về mặt cấu trúc. 
5. Nếu vị trí này hoạt động lần đầu tiên, hãy đánh dấu vị trí đó là hoạt động và quyết định xem nó ảnh hưởng như thế nào đến khả năng kết nối. 

Nếu cả hai hàng xóm đều không hoạt động, vị trí này sẽ tạo ra một khối cách ly mới và tăng`segments`bởi một. 

Nếu có chính xác một hàng xóm đang hoạt động, nó sẽ gắn vào khối đó và không thay đổi số lượng. 

Nếu cả hai khối lân cận đều hoạt động, nó sẽ hợp nhất hai khối hiện có thành một, giảm`segments`bởi một. 
6. Đối với mỗi truy vấn loại 0, xuất giá trị hiện tại của`segments`. 

Bất biến quan trọng là`segments`luôn bằng số thành phần liên thông trong tập chỉ số$i$như vậy$a_i > l$. Mỗi bản cập nhật duy trì tính bất biến này vì việc kích hoạt chỉ ảnh hưởng đến lân cận cục bộ. Không có vùng không hoạt động nào được tách hoặc hợp nhất trừ khi điểm ranh giới được kích hoạt và hiệu ứng đó được nắm bắt hoàn toàn bằng cách kiểm tra hai vùng lân cận. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, l = map(int, input().split())
    a = list(map(int, input().split()))

    active = [0] * n
    segments = 0

    for i in range(n):
        if a[i] > l:
            active[i] = 1
            if i == 0 or active[i - 1] == 0:
                segments += 1

    for _ in range(m):
        tmp = input().split()
        t = int(tmp[0])

        if t == 0:
            print(segments)
        else:
            p = int(tmp[1]) - 1
            d = int(tmp[2])

            was = active[p]
            a[p] += d

            if was == 0 and a[p] > l:
                active[p] = 1

                left = active[p - 1] if p > 0 else 0
                right = active[p + 1] if p < n - 1 else 0

                if left and right:
                    segments -= 1
                elif not left and not right:
                    segments += 1
                # else: merges with one side, no change

solve()
```Giải pháp bắt đầu bằng cách chuyển đổi mảng ban đầu thành mảng kích hoạt nhị phân. Sau đó, nó tính toán số lượng phân đoạn hoạt động trong một lần chuyển, sử dụng thực tế là phân đoạn mới bắt đầu chính xác khi một vị trí đang hoạt động còn vị trí trước đó thì không. 

Mỗi bản cập nhật sửa đổi tối đa một vị trí từ không hoạt động sang hoạt động. Mã kiểm tra cẩn thận quá trình chuyển đổi này trước khi cập nhật cấu trúc phân đoạn. Việc kiểm tra lân cận là đủ vì chỉ có lân cận cục bộ mới có thể thay đổi số lượng thành phần được kết nối trong đường một chiều. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu: 

Trạng thái ban đầu là$[1, 2, 3, 4]$với$l = 3$. Chỉ có chỉ mục 4 được kích hoạt nên chỉ có một phân đoạn. 

| Bước | Mảng | Mẫu hoạt động | Phân đoạn | 
| --- | --- | --- | --- | 
| bắt đầu | 1 2 3 4 | 0 0 0 1 | 1 | 
| + (2, +3) | 1 5 3 4 | 0 1 0 1 | 2 | 
| truy vấn | - | - | 2 | 
| + (1, +3) | 4 5 3 4 | 1 1 0 1 | 2 | 
| truy vấn | - | - | 2 | 
| + (3, +1) | 4 5 4 4 | 1 1 1 1 | 1 | 
| truy vấn | - | - | 1 | 

Bản cập nhật đầu tiên tạo ra một vị trí hoạt động biệt lập mới ở chỉ số 2, tạo ra hai phân đoạn. Bản cập nhật thứ hai hợp nhất phía bên trái thành một khối hiện có nhưng không thay đổi số lượng phân đoạn. Bản cập nhật cuối cùng sẽ lấp đầy khoảng trống giữa tất cả các vùng đang hoạt động, hợp nhất mọi thứ thành một khối liên tục. 

Dấu vết này cho thấy rằng chỉ có kích hoạt ranh giới mới quan trọng; tăng trưởng nội bộ trong một khu vực đã hoạt động không ảnh hưởng đến cấu trúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + m)$| Mỗi chỉ mục sẽ hoạt động tối đa một lần và mỗi truy vấn được xử lý trong thời gian không đổi | 
| Không gian |$O(n)$| Chúng tôi lưu trữ trạng thái kích hoạt cho từng vị trí | 

Thuật toán phù hợp thoải mái trong giới hạn vì mọi hoạt động đều có thời gian không đổi và tổng số thay đổi trạng thái có ý nghĩa được giới hạn bởi$n$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    import sys

    def solve():
        n, m, l = map(int, input().split())
        a = list(map(int, input().split()))

        active = [0] * n
        segments = 0

        for i in range(n):
            if a[i] > l:
                active[i] = 1
                if i == 0 or active[i - 1] == 0:
                    segments += 1

        out = []
        for _ in range(m):
            tmp = input().split()
            t = int(tmp[0])

            if t == 0:
                out.append(str(segments))
            else:
                p = int(tmp[1]) - 1
                d = int(tmp[2])

                was = active[p]
                a[p] += d

                if was == 0 and a[p] > l:
                    active[p] = 1
                    left = active[p - 1] if p > 0 else 0
                    right = active[p + 1] if p < n - 1 else 0

                    if left and right:
                        segments -= 1
                    elif not left and not right:
                        segments += 1

        return "\n".join(out)

    return solve()

# provided sample
assert run("""4 7 3
1 2 3 4
0
1 2 3
0
1 1 3
0
1 3 1
0
""") == "1\n2\n2\n1"

# all already
```
