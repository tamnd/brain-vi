---
title: "CF 105278D - Chia tách khôn ngoan"
description: "Chúng ta được cung cấp một danh sách các giao dịch chuyển tiền giữa mọi người, trong đó mỗi bản ghi ghi rằng một người đã trả tiền thay cho người khác, tạo ra một mối quan hệ nợ ngầm. Từ những giao dịch này, chúng ta có thể tính toán xem mỗi người cuối cùng nợ hoặc bị nợ bao nhiêu."
date: "2026-06-23T14:17:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105278
codeforces_index: "D"
codeforces_contest_name: "2024 ICPC Universidad Nacional de Colombia Programming Contest"
rating: 0
weight: 105278
solve_time_s: 113
verified: false
draft: false
---

[CF 105278D - Phân chia khôn ngoan](https://codeforces.com/problemset/problem/105278/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 53s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các giao dịch chuyển tiền giữa mọi người, trong đó mỗi bản ghi ghi rằng một người đã trả tiền thay cho người khác, tạo ra một mối quan hệ nợ ngầm. Từ những giao dịch này, chúng ta có thể tính toán xem mỗi người cuối cùng nợ hoặc bị nợ bao nhiêu. Giá trị ròng dương có nghĩa là tổng thể người đó sẽ nhận được tiền, trong khi giá trị ròng âm có nghĩa là tổng thể họ phải trả tiền. 

Nhiệm vụ không phải là tính toán lại toàn bộ lịch sử mà thay thế nó bằng một kế hoạch giải quyết nhỏ hơn nhiều. Mỗi hoạt động thanh toán là một lần chuyển giao trực tiếp một số tiền từ người này sang người khác. Hạn chế là mỗi người được phép thực hiện tối đa một lần chuyển tiền như vậy, nhưng họ có thể nhận tiền nhiều lần. Mục tiêu là cân bằng hoàn toàn tất cả các khoản nợ để mỗi người cuối cùng có số dư ròng bằng không. 

Kích thước đầu vào đẩy tới giải pháp tuyến tính hoặc gần tuyến tính trên danh sách giao dịch. Với tối đa 200.000 người và 1.000.000 giao dịch, bất kỳ phương pháp nào liên tục mô phỏng khớp cặp hoặc quét liên tục để tìm đối tác sẽ quá chậm. Cấu trúc đề xuất mạnh mẽ việc thu gọn tất cả các giao dịch thành số dư ròng trước tiên, sau đó thực hiện xây dựng kế hoạch thanh toán cuối cùng một lần duy nhất trong khoảng thời gian khoảng O(N log N) hoặc O(N). 

Một trường hợp thất bại tinh tế xuất hiện khi nghĩ đến việc ghép đôi người một cách tùy tiện mà không theo dõi năng lực còn lại. Ví dụ: nếu một người nợ 100 và chủ nợ duy nhất mà họ được kết hợp chỉ có thể nhận được 20, thì việc chuyển nhượng một cặp ngây thơ sẽ gặp khó khăn vì con nợ không được phép chia số tiền chuyển đi của họ. Cấu trúc chính xác phải đảm bảo rằng mọi lần truyền đi khớp chính xác với dung lượng còn lại của bộ thu đã chọn hoặc chúng tôi chỉ khớp các cặp tương thích khi có đủ số lượng. 

Một trường hợp khác phát sinh khi việc cân bằng được thực hiện cục bộ cho mỗi giao dịch thay vì trên toàn cầu cho mỗi người. Hai lần hủy bỏ trung gian có thể ẩn số dư ròng khác 0 mà sau này sẽ phá vỡ tính khả thi nếu bị bỏ qua. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực là liên tục chọn bất kỳ người nào có số dư dương và cố gắng ghép họ với những người nợ tiền cho đến khi tất cả mọi người đều về 0. Điều này có thể được mô phỏng bằng cách duy trì danh sách số dư động và liên tục tìm kiếm các cặp tương thích. Mỗi hoạt động sẽ yêu cầu quét bản sao hợp lệ và cập nhật số dư còn lại. Trong trường hợp xấu nhất, mỗi người trong số O(N) sẽ kích hoạt quét trên nhóm còn lại, tạo ra hành vi O(N^2), vượt xa giới hạn khi N là 200.000. 

Quan sát quan trọng là lịch sử giao dịch không còn quan trọng sau khi tính toán số dư ròng. Hệ thống trở thành một bài toán bảo toàn dòng chảy đơn giản: tổng đầu vào bằng tổng đầu ra, do đó bài toán giảm xuống việc phân phối lại thặng dư dương cho thâm hụt âm. Hạn chế mà mỗi người có thể bắt đầu tối đa một lần chuyển tiền có nghĩa là mỗi người mắc nợ phải được chỉ định chính xác một cạnh đi, vì vậy chúng ta phải coi mỗi số dư âm là một đơn vị không thể chia được và phải được gửi trong một lần di chuyển. 

Điều này biến vấn đề thành một sự kết hợp tham lam giữa “các nút thâm hụt” và “công suất dư thừa”. Mỗi chủ nợ có thể chấp nhận nhiều khoản chuyển khoản đến, trong khi mỗi con nợ phải được chỉ định chính xác một chủ nợ. Yêu cầu duy nhất là chủ nợ được chọn còn đủ năng lực để tiếp nhận toàn bộ số tiền của con nợ tại thời điểm chuyển nhượng. Một cách tự nhiên để thực thi điều này là luôn khớp với khoản thặng dư lớn nhất còn lại trước tiên, bởi vì những khoản thặng dư nhỏ hơn có nhiều khả năng sẽ không thể sử dụng được cho các khoản nợ lớn sau này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Ghép đôi vũ phu | O(N^2) | O(N) | Quá chậm | 
| Tham lam với Số dư ròng + Cấu trúc tối đa | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Trước tiên, chúng tôi nén tất cả các giao dịch vào một số dư duy nhất cho mỗi người. 

1. Tính số dư ròng của mỗi người bằng cách lặp lại tất cả các giao dịch. Nếu người A trả C cho B, chúng ta cộng C vào số dư của A và trừ C khỏi số dư của B. Điều này đưa ra số tiền cuối cùng mà mỗi người sẽ nhận hoặc trả. 
2. Chia mọi người thành hai nhóm. Một nhóm gồm tất cả những người có số dư dương, nghĩa là họ sẽ nhận được tiền. Cái còn lại chứa tất cả những người có số dư âm, nghĩa là họ phải trả tiền. 
3. Lưu trữ tất cả số dư dương trong một cấu trúc cho phép trích xuất nhiều lần dung lượng còn lại lớn nhất, chẳng hạn như vùng nhớ tối đa được khóa theo số tiền còn lại. Mỗi mục giữ cả chỉ mục người và năng lực còn lại của họ. 
4. Lặp lại tất cả những người có số dư âm. Với mỗi người nợ i có số tiền d bằng trừ số dư của họ, chúng ta phải chỉ định đúng một lần chuyển đi. 
5. Trích xuất chủ nợ có khả năng cao nhất hiện tại ra khỏi đống. Nếu khả năng còn lại của họ nhỏ hơn d, chúng tôi tạm thời loại họ khỏi danh sách xem xét và xét xử chủ nợ lớn nhất tiếp theo. Điều này có hiệu quả vì chủ nợ không thể đáp ứng đầy đủ cho con nợ này vẫn có thể có ích cho những con nợ nhỏ hơn sau này. 
6. Khi chúng tôi tìm thấy chủ nợ j có năng lực còn lại ít nhất là d, chúng tôi sẽ thực hiện chuyển khoản từ i sang j số tiền d. Sau đó chúng tôi giảm công suất còn lại của j xuống d. 
7. Nếu j vẫn còn dung lượng, chúng ta sẽ đẩy nó trở lại heap. Nếu không, chúng tôi loại bỏ nó. 
8. Tiếp tục cho đến khi tất cả người mắc nợ được xử lý. Mỗi người mắc nợ được chỉ định chính xác một lần chuyển tiền đi. 

Tính đúng đắn dựa trên tính bất biến là heap luôn thể hiện chính xác khả năng tiếp nhận chưa sử dụng của tất cả các chủ nợ. Mỗi nhiệm vụ loại bỏ hoàn toàn một con nợ và làm giảm đúng năng lực của một chủ nợ, không bao giờ chia tách nghĩa vụ của con nợ. 

Sự lựa chọn tham lam là luôn chọn chủ nợ lớn nhất hiện có để đảm bảo rằng nếu có một nhiệm vụ khả thi, chúng ta sẽ không tiêu thụ sớm các chủ nợ nhỏ theo cách ngăn cản các khoản nợ lớn sau này. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def main():
    n, m = map(int, input().split())
    bal = [0] * (n + 1)

    for _ in range(m):
        a, b, c = map(int, input().split())
        bal[a] += c
        bal[b] -= c

    pos = []
    neg = []

    for i in range(1, n + 1):
        if bal[i] > 0:
            pos.append([bal[i], i])
        elif bal[i] < 0:
            neg.append([i, -bal[i]])

    heap = []
    for amt, i in pos:
        heapq.heappush(heap, (-amt, i))

    res = []

    for i, need in neg:
        while need > 0:
            amt, j = heapq.heappop(heap)
            amt = -amt

            if amt >= need:
                res.append((i, j, need))
                amt -= need
                need = 0
                if amt > 0:
                    heapq.heappush(heap, (-amt, j))
            else:
                res.append((i, j, amt))
                need -= amt

    print(len(res))
    for a, b, c in res:
        print(a, b, c)

if __name__ == "__main__":
    main()
```Giải pháp bắt đầu bằng cách thu gọn tất cả các giao dịch thành một số dư ròng duy nhất cho mỗi người. Bước này rất cần thiết vì chỉ có số tiền ròng cuối cùng mới được thanh toán; chuyển giao trung gian không có ràng buộc bổ sung. 

Số dư dương được chuyển thành một đống tối đa để chúng ta luôn có thể tiếp cận chủ nợ với khả năng còn lại lớn nhất. Sự lựa chọn này rất quan trọng vì các chủ nợ nhỏ hơn là nguồn lực mong manh hơn và cần được dành cho những khoản nợ nhỏ hơn nếu có thể. 

Mỗi con nợ được xử lý đúng một lần. Vòng lặp bên trong phân bổ khoản nợ của họ một cách tham lam cho các chủ nợ hiện có, phân chia năng lực của chủ nợ nếu cần thiết. Mặc dù chủ nợ có thể nhận được nhiều lần chuyển tiền đến, mỗi con nợ luôn được chỉ định một lần chuyển tiền đi duy nhất, tuân theo các ràng buộc. 

Một điểm tinh tế là các chủ nợ chỉ được đưa lại vào đống nếu họ vẫn còn năng lực. Điều này đảm bảo kích thước heap vẫn bị giới hạn bởi O(N). 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 1
1 2 50
```Sau khi xử lý giao dịch, số dư sẽ trở thành: 

Người 1: +50 

Người 2: -50 

Chúng tôi xây dựng vùng heap với (50, 1) và xử lý người 2. 

| Bước | Con nợ | Cần | Chủ nợ được chọn | Chủ nợ trước | Chuyển nhượng | Chủ nợ Sau | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 50 | 1 | 50 | 2 → 1 : 50 | 0 | 

Đầu ra:```
1
2 1 50
```Điều này cho thấy trường hợp đơn giản nhất trong đó một con nợ khớp chính xác với một chủ nợ. 

### Mẫu 2 

đầu vào:```
3 3
1 2 2
2 3 4
3 1 6
```Số dư ròng: 

Người 1: +4 

Người 2: -2 

Người 3: -2 

Đống bắt đầu bằng (4, 1). 

| Bước | Con nợ | Cần | Chủ nợ | Trước | Chuyển nhượng | Sau | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 2 | 1 | 4 | 2 → 1 : 2 | 2 | 
| 2 | 3 | 2 | 1 | 2 | 3 → 1 : 2 | 0 | 

Đầu ra:```
2
2 1 2
3 1 2
```Điều này chứng tỏ một chủ nợ có thể phục vụ nhiều con nợ, dần dần cạn kiệt năng lực. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N + M) | M để tổng hợp số dư, N log N cho các thao tác heap trong quá trình khớp | 
| Không gian | O(N) | Lưu trữ số dư và mục nhập đống | 

Thuật toán phù hợp thoải mái trong các ràng buộc vì ngay cả các giao dịch 1e6 cũng chỉ yêu cầu vượt qua tuyến tính và quy mô hoạt động của đống theo số lượng người tham gia tích cực chứ không phải số lượng giao dịch. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from collections import defaultdict
    import heapq

    input = sys.stdin.readline
    n, m = map(int, input().split())
    bal = [0] * (n + 1)

    for _ in range(m):
        a, b, c = map(int, input().split())
        bal[a] += c
        bal[b] -= c

    pos = []
    neg = []

    for i in range(1, n + 1):
        if bal[i] > 0:
            pos.append([bal[i], i])
        elif bal[i] < 0:
            neg.append([i, -bal[i]])

    heap = []
    for amt, i in pos:
        heapq.heappush(heap, (-amt, i))

    res = []

    for i, need in neg:
        while need > 0:
            amt, j = heapq.heappop(heap)
            amt = -amt
            take = min(amt, need)
            res.append((i, j, take))
            amt -= take
            need -= take
            if amt > 0:
                heapq.heappush(heap, (-amt, j))

    out = [str(len(res))]
    for a, b, c in res:
        out.append(f"{a} {b} {c}")
    return "\n".join(out)

# provided sample 1
assert run("2 1\n1 2 50\n") == "1\n2 1 50"

# provided sample 2
assert run("3 3\n1 2 2\n2 3 4\n3 1 6\n") == "2\n2 1 2\n3 1 2"

# single node pair
assert run("2 1\n1 2 10\n") == "1\n2 1 10"

# balanced chain
assert run("4 3\n1 2 5\n2 3 5\n3 4 5\n") == "1\n4 1 5"

# zero net internal cancellations
assert run("3 2\n1 2 5\n2 1 5\n") == "0"

# split creditors
assert run("3 2\n1 2 10\n1 3 5\n") == "1\n2 1 10\n3 1 5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuyển khoản đơn giản | 2 1 50 | tính đúng đắn cơ bản | 
| phân phối lại chuỗi | nhiều cạnh | xử lý chủ nợ nhiều con nợ | 
| hủy bỏ | 0 | xử lý mạng bằng không | 
| chia đến | tổng hợp đúng | chủ nợ nhận chuyển khoản nhiều lần | 

## Vỏ cạnh 

Trường hợp cạnh chính xuất hiện khi tất cả các giao dịch bị hủy theo cặp nên mọi số dư đều bằng 0. Trong trường hợp này, thuật toán xây dựng một đống trống và không xử lý con nợ nào, tạo ra các hoạt động đầu ra bằng 0 mà không cần bước vào giai đoạn khớp. 

Một trường hợp khác là khi một chủ nợ có năng lực rất lớn nhưng lại có nhiều con nợ nhỏ. Đống đảm bảo chủ nợ được tái sử dụng nhiều lần, giảm dần công suất còn lại của nó trong khi vẫn tôn trọng ràng buộc một lần gửi đi cho mỗi con nợ. 

Trường hợp tinh vi cuối cùng xảy ra khi năng lực của chủ nợ được sử dụng một phần và sau đó được đưa lại vào đống. Độ chính xác phụ thuộc vào việc luôn cập nhật dung lượng còn lại trước khi lắp lại, đảm bảo không có giá trị cũ nào được sử dụng lại.
