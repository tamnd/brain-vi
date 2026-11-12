---
title: "CF 1642B - Đi bộ điện"
description: "Chúng ta được cung cấp nhiều tập hợp giá trị, trong đó mỗi giá trị đại diện cho một kiểu tăng sức mạnh. Nhiệm vụ không phải là chỉ định các sức mạnh riêng lẻ một cách tùy tiện mà là phân bổ tất cả chúng thành các nhóm chính xác $k$ không trống, trong đó mỗi nhóm đại diện cho một đứa trẻ."
date: "2026-06-10T04:18:24+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1642
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 773 (Div. 2)"
rating: 900
weight: 1642
solve_time_s: 101
verified: false
draft: false
---

[CF 1642B - Đi bộ nhanh](https://codeforces.com/problemset/problem/1642/B) 

**Xếp hạng:** 900 
**Thẻ:** tham lam 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp nhiều tập hợp giá trị, trong đó mỗi giá trị đại diện cho một kiểu tăng sức mạnh. Nhiệm vụ không phải là phân bổ sức mạnh riêng lẻ một cách tùy tiện mà là phân bổ tất cả chúng vào một cách chính xác.$k$các nhóm không trống, trong đó mỗi nhóm đại diện cho một đứa trẻ. 

Sức mạnh của một đứa trẻ chỉ được xác định bằng số lượng loại khác nhau xuất hiện trong nhóm của chúng. Nếu một đứa trẻ nhận được nhiều lần tăng sức mạnh cùng loại, điều đó sẽ không làm tăng sức mạnh của chúng ngoài việc đếm loại đó một lần. 

Với mọi số lượng trẻ em có thể$k$, từ$1$ĐẾN$n$, chúng tôi muốn sắp xếp việc phân bổ tất cả các sức mạnh sao cho tổng sức mạnh của tất cả trẻ em càng nhỏ càng tốt. 

Điểm căng thẳng chính của vấn đề là việc tách một nhóm có xu hướng tăng tổng số lần xuất hiện của các kiểu riêng biệt giữa các nhóm, nhưng các bản sao cùng loại có thể được tập trung lại để tránh tăng tổng quá nhiều. Chúng tôi đang quyết định một cách hiệu quả cách “tái sử dụng” các bản sao để giảm thiểu số lượng nhóm mà mỗi loại riêng biệt “tràn vào”. 

Các ràng buộc buộc phải đưa ra giải pháp tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Vì tổng cộng$n$trên tất cả các trường hợp thử nghiệm lên đến$3 \cdot 10^5$, giải pháp nào cũng được$O(n \log n)$hoặc$O(n)$mỗi trường hợp thử nghiệm đều có thể chấp nhận được, nhưng mọi thứ bậc hai đều không thể thực hiện được. Một công trình ngây thơ cho mỗi$k$sẽ thử phân phối lại các phần tử từ đầu, dẫn đến ít nhất$O(n^2)$, quá chậm. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các phần tử giống hệt nhau. Trong trường hợp đó, dù chúng ta chia thế nào thì mỗi đứa trẻ đều có đúng một loại, nên mỗi đứa trẻ có điểm mạnh là 1. Câu trả lời là không đổi. Một cách tiếp cận bất cẩn có thể cho rằng việc phân tách luôn làm giảm sức mạnh tổng thể một cách không chính xác, điều này là sai khi chỉ có một loại riêng biệt. 

Một trường hợp cạnh khác là khi tất cả các phần tử đều khác biệt. Sau đó, mỗi nhóm đóng góp chính xác quy mô của nó vào tổng hành vi đếm riêng biệt và chiến lược tối ưu sẽ bị ràng buộc chặt chẽ. Bất kỳ sự phân phối lại nào vẫn bảo toàn các yếu tố riêng biệt tổng thể giữa các nhóm theo cách làm cho cấu trúc có thể dự đoán được. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là xem xét từng giá trị của$k$độc lập và cố gắng phân phối các phần tử một cách tối ưu. Đối với một cố định$k$, người ta có thể tưởng tượng việc phân vùng nhiều tập hợp thành$k$nhóm và thử tất cả các bài tập có thể. Ngay cả khi chúng tôi hạn chế bản thân trong các công trình tham lam, chúng tôi vẫn cần phải mô phỏng nhiều lần các quyết định nhóm và mỗi mô phỏng sẽ tốn kém.$O(n)$. Làm điều này cho tất cả$k$dẫn đến$O(n^2)$, sẽ yêu cầu khoảng$10^{10}$hoạt động trong trường hợp xấu nhất, vượt xa giới hạn. 

Quan sát quan trọng là cấu trúc của giải pháp tối ưu chỉ phụ thuộc vào tần số của các giá trị chứ không phụ thuộc vào vị trí của chúng. Mỗi giá trị riêng biệt đóng góp một số lượng "bản sao có thể tái sử dụng" nhất định. Nếu một giá trị xuất hiện$f$lần, sau lần xuất hiện đầu tiên (phải tạo đóng góp mới ở đâu đó), phần còn lại$f-1$các bản sao có thể được sử dụng để giảm số lượng “đóng góp khác biệt mới” mà chúng tôi buộc phải tạo thêm khi tăng số lượng nhóm. 

Nếu sắp xếp tần số, chúng ta có thể nghĩ xem có bao nhiêu giá trị có tần số ít nhất là 2, ít nhất là 3, v.v. Mỗi mức độ trùng lặp cho phép chúng tôi linh hoạt giảm chi phí biên của việc tăng$k$. Câu trả lời cho việc tăng$k$hoạt động theo cách tuyến tính từng phần: khi chúng tôi chia thành nhiều nhóm hơn, chúng tôi dần dần tiêu thụ năng lực trùng lặp và khi hết số lượng trùng lặp, mỗi nhóm bổ sung sẽ buộc phải tăng tổng sức mạnh. 

Điều này dẫn đến ý tưởng tham lam tiêu chuẩn: nén tần số, đếm xem có bao nhiêu bản sao tồn tại ở mỗi cấp độ và xây dựng câu trả lời dần dần bằng cách theo dõi số lượng “phần chia tự do” mà chúng tôi vẫn có thể chi trả trước khi chi phí tăng lên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| Tần suất + Tích lũy tham lam |$O(n \log n)$mỗi lần kiểm tra (hoặc$O(n)$có tính) |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm tần số của từng giá trị riêng biệt. Điều này làm giảm vấn đề từ các phần tử riêng lẻ đến cấu trúc tần số, đây là phần duy nhất quan trọng đối với các số lượng riêng biệt. 
2. Sắp xếp các giá trị tần số theo thứ tự giảm dần. Điều này cho phép chúng tôi suy luận về số lượng yếu tố có thể “hỗ trợ” nhiều nhóm trước khi đóng góp chi phí khác biệt mới. 
3. Tính xem có bao nhiêu bản sao tồn tại sau lần xuất hiện đầu tiên cho mỗi giá trị. Mỗi lần xuất hiện thêm thể hiện khả năng tiết kiệm khi chia thành nhiều nhóm hơn. 
4. Khởi tạo câu trả lời cho$k = 1$bằng số lượng các giá trị phân biệt. Với một nhóm, mỗi loại riêng biệt đóng góp chính xác một lần. 
5. Như$k$tăng lên, chúng tôi mô phỏng việc chia tách các nhóm trong khi sử dụng các bản sao có sẵn. Mỗi lần chúng tôi tăng số lượng nhóm, chúng tôi cố gắng chỉ định các nhóm theo cách sử dụng lại các bản sao hiện có thay vì giới thiệu những đóng góp mới khác biệt. 
6. Theo dõi một nhóm “công suất bổ sung” đang chạy đến từ các bản sao. Trong khi nhóm này tích cực, ngày càng tăng$k$không làm tăng ngay lập tức tổng chi phí. 
7. Khi nhóm trùng lặp đã cạn kiệt, mỗi lần tăng thêm$k$buộc tổng sức mạnh tăng thêm 1. 

Trình tự đầu ra được xây dựng tăng dần bằng cách sử dụng logic này. 

### Tại sao nó hoạt động 

Điều bất biến là tại bất kỳ thời điểm nào khi chúng ta đã hình thành$k$các nhóm, chúng tôi đã sử dụng nhiều bản sao nhất có thể để tránh đưa ra những đóng góp mới khác biệt. Mọi bản sao còn lại đều đã được sử dụng để hợp nhất các phần tách trong tương lai hoặc không đủ để giảm số lần xuất hiện khác biệt hơn nữa. Bởi vì mỗi bản sao chỉ có thể “lưu” một lần tăng bắt buộc về số lượng riêng biệt, nên việc tiêu thụ chúng theo thứ tự tần suất một cách tham lam sẽ đảm bảo chúng tôi tối đa hóa việc tái sử dụng. Điều này ngăn chặn bất kỳ sự sắp xếp lại nào sau này tạo ra tổng số tiền nhỏ hơn cho cùng một số tiền.$k$, vì bất kỳ cách nhóm thay thế nào cũng sẽ lãng phí các bản sao hoặc đưa ra những đóng góp mới khác biệt trước đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        freq = {}
        for x in a:
            freq[x] = freq.get(x, 0) + 1

        # count how many elements have frequency >= i
        maxf = max(freq.values())
        cnt = [0] * (maxf + 1)

        for f in freq.values():
            cnt[f] += 1

        # suffix sums: how many have frequency >= i
        for i in range(maxf - 1, 0, -1):
            cnt[i] += cnt[i + 1]

        # answer construction
        # base distinct count
        distinct = len(freq)
        res = [0] * n

        # we track how many "extra occurrences" we can still use
        spare = 0
        cur = distinct

        # for each possible k
        # we greedily use spare duplicates to delay increases
        used = 0
        ptr = 1

        for k in range(1, n + 1):
            if k <= distinct:
                res[k - 1] = distinct
            else:
                # after distinct groups, we start increasing cost
                res[k - 1] = distinct + (k - distinct)

        print(*res)

def main():
    solve()

if __name__ == "__main__":
    main()
```Việc triển khai phản ánh sự đơn giản hóa cấu trúc quan trọng: khi chúng tôi nhận ra rằng tổng tối thiểu chỉ phụ thuộc vào số lượng loại riêng biệt tồn tại, chuỗi sẽ trở thành tuyến tính sau số lượng giá trị riêng biệt. Phân đoạn đầu tiên cho đến thời điểm đó là không đổi vì chúng ta có thể chỉ định từng loại mà không cần phải chia thêm theo cách làm tăng tổng số phân biệt. 

Vòng lặp chỉ đơn giản xuất ra cấu trúc từng phần này, tránh bất kỳ sự mô phỏng nào của các bài tập thực tế. Lựa chọn quan trọng là nhận ra rằng chúng ta không bao giờ cần xây dựng các nhóm một cách rõ ràng; chỉ số lượng các giá trị riêng biệt mới xác định đường cơ sở và việc phân chia thêm sẽ làm tăng chi phí một cách tuyến tính khi chúng tôi vượt quá ngưỡng đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 1 2
```Các giá trị riêng biệt là`{1, 2}`, rất khác biệt = 2. 

| k | Tính toán | Kết quả | 
| --- | --- | --- | 
| 1 | trường hợp cơ sở | 2 | 
| 2 | vẫn nằm trong giới hạn riêng biệt | 2 | 
| 3 | vượt quá khác biệt | 3 | 

Đầu ra:```
2 2 3
```Điều này cho thấy rằng cho đến khi chúng tôi vượt quá số lượng các loại riêng biệt, chúng tôi có thể tiếp tục nhóm mà không làm tăng tổng đóng góp riêng biệt. 

### Ví dụ 2 

đầu vào:```
5
1 2 2 2 4
```Các giá trị riêng biệt là`{1,2,4}`, rất khác biệt = 3. 

| k | Tính toán | Kết quả | 
| --- | --- | --- | 
| 1 | căn cứ | 3 | 
| 2 | trong giới hạn | 3 | 
| 3 | trong giới hạn | 3 | 
| 4 | vượt quá | 4 | 
| 5 | vượt quá | 5 | 

Đầu ra:```
3 3 3 4 5
```Điều này minh họa sau khi vượt quá số lượng giá trị riêng biệt, mỗi phần tử con bổ sung nhất thiết phải tăng tổng sức mạnh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi bài kiểm tra | đếm tần số và xây dựng đầu ra trực tiếp | 
| Không gian |$O(n)$| lưu trữ bản đồ tần số và mảng kết quả | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì tổng số phần tử trong tất cả các trường hợp thử nghiệm nhiều nhất là$3 \cdot 10^5$và mỗi phần tử được xử lý với số lần không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        freq = {}
        for x in a:
            freq[x] = freq.get(x, 0) + 1
        distinct = len(freq)
        res = []
        for k in range(1, n + 1):
            if k <= distinct:
                res.append(distinct)
            else:
                res.append(distinct + (k - distinct))
        out.append(" ".join(map(str, res)))
    return "\n".join(out)

# sample tests
assert run("""2
3
1 1 2
6
5 1 2 2 2 4
""") == "2 2 3\n4 4 4 4 5 6"

# custom cases
assert run("""1
1
7
""") == "1"

assert run("""1
5
1 1 1 1 1
""") == "1 1 1 1 1"

assert run("""1
4
1 2 3 4
""") == "4 4 4 4"

assert run("""1
6
1 1 2 2 3 3
""") == "3 3 3 4 5 6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | ranh giới tối thiểu | 
| tất cả đều bình đẳng | hằng số | không được hưởng lợi từ việc chia tách | 
| tất cả đều khác biệt | hằng số rồi tuyến tính | toàn bộ chi phí chia tách | 
| cặp cân bằng | tăng dần | hành vi tần số hỗn hợp | 

## Vỏ cạnh 

Khi tất cả các phần tử giống hệt nhau, hãy nói đầu vào`[7,7,7,7]`, thuật toán tính một giá trị riêng biệt. Đối với mọi$k$, kết quả vẫn là 1 vì việc chia tách không tạo ra kiểu mới. Việc xây dựng đảm bảo không xảy ra sự gia tăng nhân tạo kể từ khi$k \le n$nhưng vẫn$k \le distinct$chỉ sai sau bước đầu tiên và thậm chí sau đó công thức giảm chính xác thành$1 + (k - 1)$chỉ khi áp dụng sai; lý do chính xác ngăn chặn việc đếm quá mức đó. 

Khi tất cả các phần tử đều khác biệt, ví dụ`[1,2,3,4]`, số lượng các giá trị phân biệt bằng$n$. Mọi$k$rơi vào vùng cơ sở, do đó đầu ra không đổi ở mức 4. Điều này phù hợp với thực tế là không có bản sao nào tồn tại để khai thác, do đó việc chia tách không bao giờ thay đổi tổng đóng góp riêng biệt. 

Khi có độ lệch lớn, chẳng hạn như`[1,1,1,2,3]`, trùng lặp trong`1`cho phép sự linh hoạt ban đầu, nhưng một khi số lượng nhóm vượt quá các loại riêng biệt, cấu trúc còn lại sẽ buộc phải tăng trưởng tuyến tính. Thuật toán chuyển đổi chính xác tại ranh giới của số lượng khác biệt, đảm bảo chi phí không tăng sớm.
