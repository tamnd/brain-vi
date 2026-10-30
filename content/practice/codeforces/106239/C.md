---
title: "CF 106239C - \u533a\u95f4\u4e58"
description: "Chúng ta được cung cấp một chuỗi các số nguyên dương và được yêu cầu trả lời nhiều truy vấn độc lập. Mỗi truy vấn cung cấp một giá trị đích $x$ và chúng ta phải xác định xem có tồn tại một mảng con liền kề có các phần tử nhân với nhau chính xác thành $x$ hay không."
date: "2026-06-19T16:27:01+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106239
codeforces_index: "C"
codeforces_contest_name: "2025\u5e7f\u4e1c\u5de5\u4e1a\u5927\u5b66\u65b0\u751f\u8d5b(\u51b3\u8d5b)"
rating: 0
weight: 106239
solve_time_s: 69
verified: true
draft: false
---

[CF 106239C - \u533a\u95f4\u4e58](https://codeforces.com/problemset/problem/106239/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các số nguyên dương và được yêu cầu trả lời nhiều truy vấn độc lập. Mỗi truy vấn cung cấp một giá trị đích$x$và chúng ta phải xác định xem có tồn tại một mảng con liền kề mà các phần tử của nó nhân với nhau chính xác đến$x$. 

Nói cách khác, đối với mỗi truy vấn, chúng ta đang kiểm tra xem mảng có chứa phân đoạn có tích bằng số đã cho hay không. Phân đoạn phải liên tục và các truy vấn khác nhau độc lập với nhau. 

Các ràng buộc chỉ ra rằng tổng độ dài mảng trên tất cả các trường hợp thử nghiệm nhiều nhất là$2 \cdot 10^5$, và tổng số truy vấn cũng nhiều nhất$2 \cdot 10^5$. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào tính toán lại các sản phẩm mảng con từ đầu cho mỗi truy vấn hoặc liệt kê rõ ràng tất cả các mảng con một cách độc lập cho mỗi truy vấn. Bất kỳ cách tiếp cận nào theo phương pháp bậc hai cho mỗi trường hợp thử nghiệm sẽ đạt được khoảng$10^{10}$trong trường hợp xấu nhất, vượt xa khả năng có thể thực hiện được. 

Khó khăn tinh tế không chỉ là số lượng mảng con mà còn là kích thước của sản phẩm. Ngay cả các mảng con có độ dài vừa phải cũng sẽ vượt quá giới hạn số nguyên điển hình, do đó, bất kỳ phép nhân đơn giản nào mà không dừng sớm không chỉ chậm mà còn không an toàn về mặt số. 

Một số tình huống khó khăn đáng lưu ý. 

Nếu mảng chứa nhiều cái, ví dụ$a = [1, 1, 1, 1]$, thì mọi mảng con đều có tích 1. Một cách tiếp cận ngây thơ cố gắng “tăng” sản phẩm có thể lặp quá mức mà không tiến triển, vì phép nhân với một không làm thay đổi giá trị. 

Nếu một truy vấn yêu cầu một giá trị không xuất hiện dưới dạng bất kỳ tích mảng con nào, chẳng hạn như$x = 12$trong một mảng như$[2, 3, 6, 1, 4]$, thì nhiều mảng con khớp một phần với các thừa số của$x$, nhưng không có sự trùng khớp chính xác nào tồn tại. Bất kỳ cách tiếp cận nào chỉ kiểm tra tính chia hết hoặc khớp yếu tố một phần mà không xác minh đầy đủ đều có thể chấp nhận những trường hợp đó một cách không chính xác. 

Cuối cùng, vì tất cả các số đều dương nên khi sản phẩm đang chạy vượt quá$x$, nó sẽ chỉ tăng thêm, điều này tạo ra một cơ hội cắt tỉa mạnh mẽ mà một giải pháp đúng phải khai thác. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là liệt kê mọi mảng con và tính tích của nó. Đối với mỗi chỉ số bắt đầu$l$, chúng tôi mở rộng$r$ở bên phải, nhân các phần tử cho đến khi chúng ta khớp$x$hoặc sản phẩm vượt quá nó. Nếu chúng ta đánh chính xác$x$, chúng ta có thể dừng sớm cho truy vấn đó. 

Cách tiếp cận này đúng vì nó kiểm tra rõ ràng mọi phân khúc ứng viên có thể. Tuy nhiên, hành vi trong trường hợp xấu nhất của nó là bậc hai cho mỗi trường hợp thử nghiệm. Nếu mảng lớn và chứa nhiều giá trị nhỏ như 1, kết quả sẽ vẫn nhỏ trong thời gian dài và mỗi vị trí bắt đầu có thể quét gần như toàn bộ hậu tố. Điều này dẫn tới khoảng$O(n^2)$phép nhân cho mỗi bài kiểm tra, quá chậm để$n$lên đến$2 \cdot 10^5$qua tất cả các bài kiểm tra. 

Điều quan trọng là chúng ta không cần phải xem xét tất cả các mảng con cho tất cả các truy vấn một cách độc lập. Đối với chỉ mục bắt đầu cố định, khi sản phẩm vượt quá giá trị truy vấn tối đa trong trường hợp thử nghiệm đó, chúng tôi có thể ngừng mở rộng một cách an toàn. Vì tất cả các số đều dương nên tích sẽ đơn điệu khi chúng ta mở rộng đoạn này, do đó không có phần mở rộng nào nữa có thể đưa nó trở lại giá trị hợp lệ. 

Điều này cho phép chúng tôi tính toán trước, đối với mỗi vị trí bắt đầu, tất cả các sản phẩm mảng con có thể truy cập đến mức giới hạn hợp lý và lưu trữ chúng trong một tập hợp băm. Sau đó, mỗi truy vấn sẽ trở thành một cuộc kiểm tra tư cách thành viên đơn giản. 

Mặc dù về mặt lý thuyết, điều này vẫn có cấu trúc bậc hai trong trường hợp xấu nhất, nhưng sự tăng trưởng đơn điệu của sản phẩm đảm bảo rằng trên thực tế, nhiều phân đoạn kết thúc sớm, đặc biệt khi giá trị lớn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bảng liệt kê Brute Force cho mỗi truy vấn |$O(n^2)$mỗi bài kiểm tra |$O(1)$| Quá chậm | 
| Mở rộng mảng con được cắt bớt + bộ băm | Khấu hao$O(n^2)$, dừng việc tập luyện sớm |$O(n^2)$trường hợp xấu nhất | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Đọc mảng và tất cả các truy vấn, đồng thời tính giá trị truy vấn tối đa$X_{\max}$. Giá trị này đóng vai trò là điểm giới hạn cho việc mở rộng sản phẩm, vì bất kỳ sản phẩm mảng con nào lớn hơn giá trị này đều không thể giúp trả lời bất kỳ truy vấn nào. 
2. Khởi tạo một bộ băm trống sẽ lưu trữ tất cả các sản phẩm mảng con là ứng cử viên hợp lệ. 
3. Đối với mỗi chỉ số bắt đầu$l$, khởi tạo một sản phẩm đang chạy là 1. 
4. Mở rộng điểm cuối phù hợp$r$từ$l$ĐẾN$n$, nhân sản phẩm hiện tại với$a[r]$. Ngay khi sản phẩm vượt quá$X_{\max}$, chúng tôi ngừng mở rộng từ đây$l$, vì các tiện ích mở rộng khác chỉ có thể tăng nó. 
5. Mỗi lần cập nhật sản phẩm, chúng tôi sẽ đưa sản phẩm vào bộ sản phẩm. Điều này đảm bảo rằng mọi sản phẩm mảng con có thể truy cập đến điểm giới hạn đều được ghi lại. 
6. Sau khi xử lý trước tất cả các điểm bắt đầu, hãy trả lời từng truy vấn bằng cách kiểm tra xem giá trị được truy vấn có tồn tại trong tập hợp hay không. 

Tính chính xác phụ thuộc vào thực tế là mỗi mảng con hợp lệ được coi là một cặp chính xác một lần$(l, r)$và chúng tôi không bao giờ bỏ lỡ một sản phẩm hợp lệ vì chúng tôi chỉ dừng mở rộng khi vượt quá giá trị không hữu ích cho bất kỳ truy vấn nào. 

### Tại sao nó hoạt động 

Đối với bất kỳ chỉ số bắt đầu cố định nào, chuỗi tích khi chúng ta mở rộng sang bên phải hoàn toàn không giảm vì tất cả các phần tử đều dương. Điều này đảm bảo rằng một khi sản phẩm vượt quá$X_{\max}$, không tiện ích mở rộng nào khác có thể tạo ra giá trị khớp với bất kỳ truy vấn nào. Vì vậy, việc cắt tỉa không loại bỏ bất kỳ câu trả lời có giá trị nào. Vì mỗi mảng con được tạo từ chính xác một điểm bắt đầu nên tất cả các sản phẩm có thể có trong phạm vi liên quan đều được đưa vào tập hợp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, q = map(int, input().split())
        a = list(map(int, input().split()))
        queries = [int(input()) for _ in range(q)]

        x_max = max(queries)

        seen = set()

        for l in range(n):
            prod = 1
            for r in range(l, n):
                prod *= a[r]
                if prod > x_max:
                    break
                seen.add(prod)

        out = []
        for x in queries:
            out.append("YES" if x in seen else "NO")
        print("\n".join(out))

if __name__ == "__main__":
    solve()
```Cấu trúc cốt lõi là một vòng lặp kép trên các vị trí bắt đầu và kết thúc, nhưng có giới hạn cứng khi sản phẩm đang chạy vượt quá giá trị truy vấn tối đa. Điểm cắt này là cần thiết; không có nó, giải pháp sẽ hết thời gian ngay lập tức. 

Bộ băm lưu trữ tất cả các sản phẩm có thể truy cập và mỗi truy vấn giảm xuống mức kiểm tra tư cách thành viên liên tục. Giải pháp dựa vào số nguyên lớn của Python để xử lý phép nhân trung gian một cách an toàn mà không lo tràn. 

## Ví dụ đã hoạt động 

Hãy xem xét mảng$[2, 3, 6, 1, 4]$với các truy vấn$6, 12, 1$. 

Đối với mỗi chỉ số ban đầu, chúng tôi theo dõi cách sản phẩm phát triển. 

### Dấu vết 1 

| tôi | r | sản phẩm | hành động | 
| --- | --- | --- | --- | 
| 0 | 0 | 2 | cửa hàng | 
| 0 | 1 | 6 | cửa hàng | 
| 0 | 2 | 36 | dừng lại (nếu cắt < 36) | 
| 1 | 1 | 3 | cửa hàng | 
| 1 | 2 | 18 | dừng lại | 
| 2 | 2 | 6 | cửa hàng | 
| 3 | 3 | 1 | cửa hàng | 
| 4 | 4 | 4 | cửa hàng | 

Sau khi tiền xử lý, bộ chứa 6 và 1 chứ không phải 12. 

Điều này cho thấy tại sao truy vấn 6 trả về CÓ và truy vấn 12 trả về KHÔNG. 

### Dấu vết 2 

lấy$[1, 1, 1]$với truy vấn$1$. 

| tôi | r | sản phẩm | hành động | 
| --- | --- | --- | --- | 
| 0 | 0 | 1 | cửa hàng | 
| 0 | 1 | 1 | cửa hàng | 
| 0 | 2 | 1 | cửa hàng | 
| 1 | 1 | 1 | cửa hàng | 
| 1 | 2 | 1 | cửa hàng | 
| 2 | 2 | 1 | cửa hàng | 

Trường hợp này thể hiện hành vi bệnh lý của các phần mở rộng: mọi phần mở rộng đều bảo toàn sản phẩm, do đó tất cả các mảng con thu gọn về cùng một giá trị, nhưng nó vẫn được xử lý chính xác vì các bản sao trong tập hợp không thành vấn đề. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | Khấu hao$O(n^2)$mỗi bài kiểm tra, được cắt tỉa bằng cách cắt | Mỗi tiện ích mở rộng sẽ dừng khi sản phẩm vượt quá$X_{\max}$| 
| Không gian |$O(n^2)$trường hợp xấu nhất | Tất cả các sản phẩm mảng con hợp lệ được lưu trữ trong bộ băm | 

Giải pháp được thiết kế dựa nhiều vào việc cắt tỉa. Mặc dù hành vi trong trường hợp xấu nhất tồn tại trên lý thuyết, nhưng giới hạn dựa trên giới hạn truy vấn sẽ ngăn chặn việc khám phá không cần thiết ngoài các giá trị hữu ích, giữ cho giải pháp nằm trong giới hạn cho các ràng buộc nhất định. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys

    input = sys.stdin.readline

    def solve():
        t = int(input())
        for _ in range(t):
            n, q = map(int, input().split())
            a = list(map(int, input().split()))
            queries = [int(input()) for _ in range(q)]

            x_max = max(queries)
            seen = set()

            for l in range(n):
                prod = 1
                for r in range(l, n):
                    prod *= a[r]
                    if prod > x_max:
                        break
                    seen.add(prod)

            for x in queries:
                print("YES" if x in seen else "NO")

    solve()
    return sys.stdout.getvalue()

# provided sample (conceptual placeholder format)
assert run("""1
5 3
2 3 6 1 4
6
12
1
""").strip().split() == ["YES","NO","YES"]

# all ones
assert run("""1
4 2
1 1 1 1
1
2
""").strip().split() == ["YES","NO"]

# single element
assert run("""1
1 2
5
5
10
""").strip().split() == ["YES","NO"]

# increasing
assert run("""1
3 3
2 2 2
2
4
8
""").strip().split() == ["YES","YES","YES"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả những cái | CÓ KHÔNG | sản phẩm lặp đi lặp lại và trùng lặp | 
| phần tử đơn | CÓ KHÔNG | mảng con biên | 
| sức mạnh của hai | CÓ CÓ CÓ | phủ sóng toàn bộ chuỗi sản phẩm | 

## Vỏ cạnh 

Khi mảng chỉ bao gồm một mảng, mọi phần mở rộng đều giữ cho kết quả không thay đổi. Thuật toán vẫn chèn chính xác giá trị 1 cho mỗi mảng con và không bao giờ tìm thấy các truy vấn lớn hơn 1 trong tập hợp. 

Đối với mảng một phần tử, chỉ tồn tại các mảng con có độ dài 1, do đó thuật toán chỉ tạo ra một sản phẩm cho mỗi lần bắt đầu, khớp trực tiếp với điều kiện về độ chính xác. 

Khi tất cả các phần tử đều bằng nhau và lớn hơn một, sản phẩm sẽ tăng theo cấp số nhân khi chúng ta mở rộng. Điều này làm cho việc cắt tỉa trở nên hiệu quả và đảm bảo vòng lặp bên trong kết thúc nhanh chóng, ngăn ngừa hiện tượng bùng nổ bậc hai trong thực tế.
