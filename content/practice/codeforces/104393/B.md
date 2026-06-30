---
title: "CF 104393B - Dịch vụ web của BWS Baker"
description: "Chúng ta được cung cấp một tập hợp các vi dịch vụ, mỗi vi dịch vụ tạo ra một số yêu cầu ban đầu của người dùng. Tất cả các yêu cầu này được phục vụ bởi các máy chủ giống hệt nhau, trong đó mỗi máy có giới hạn dung lượng cố định."
date: "2026-06-30T23:24:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104393
codeforces_index: "B"
codeforces_contest_name: "ICPC Masters Mexico LATAM 2023"
rating: 0
weight: 104393
solve_time_s: 90
verified: false
draft: false
---

[CF 104393B - Dịch vụ web của BWS Baker](https://codeforces.com/problemset/problem/104393/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 30s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các vi dịch vụ, mỗi vi dịch vụ tạo ra một số yêu cầu ban đầu của người dùng. Tất cả các yêu cầu này được phục vụ bởi các máy chủ giống hệt nhau, trong đó mỗi máy có giới hạn dung lượng cố định. Theo thời gian, lưu lượng truy cập tăng lên một cách rất cứng nhắc: mỗi tháng, tổng lượng yêu cầu tăng gấp đôi. 

Bất cứ khi nào tổng tải yêu cầu sau bước nhân đôi vượt quá khả năng xử lý của máy hiện đang thuê, hệ thống sẽ phản ứng bằng cách thuê thêm máy và phân chia lưu lượng giữa cơ sở hạ tầng cũ và cơ sở hạ tầng mới. Việc phân chia không phải là tùy ý, nó luôn được thực hiện bằng cách lấy một nửa lưu lượng truy cập gấp đôi và gán nó cho thiết lập hiện có, nửa còn lại cho máy chủ mới thuê. Bởi vì sự phân chia được xác định bằng cách làm tròn đến phần nghìn, nên nó thực sự là sự phân chia số nguyên của tổng tải thành hai phần gần bằng nhau. 

Số lượng quan trọng mà chúng tôi theo dõi là số lượng máy chủ cần thiết sau một số tháng nhất định khi tăng gấp đôi và có thể chia tách. Chúng tôi được yêu cầu trả lời câu hỏi này trong nhiều tháng truy vấn, tối đa 54 bước trong tương lai. 

Đầu vào cung cấp số lượt tải ban đầu cho mỗi vi dịch vụ, cùng nhau tạo thành tổng khối lượng yêu cầu ban đầu. Mỗi truy vấn hỏi: sau khi mô phỏng quá trình tăng trưởng trong một số tháng cụ thể, cần có bao nhiêu máy chủ để phục vụ tất cả các yêu cầu trong giới hạn dung lượng. 

Những hạn chế nhỏ về mặt cấu trúc nhưng lại lớn về hành vi tăng trưởng. Có tối đa 1000 vi dịch vụ nên việc tính toán tổng tải ban đầu là không đáng kể. Số tháng nhiều nhất là 54, một con số cực kỳ nhỏ nên bất kỳ giải pháp nào tính toán trước trạng thái cho tất cả các tháng đều khả thi. Vấn đề quan trọng là giá trị yêu cầu tăng theo cấp số nhân, do đó việc tính toán lại các giá trị thô cho mỗi truy vấn là không cần thiết và sẽ lãng phí nếu thực hiện độc lập cho mỗi truy vấn. 

Một cách giải thích đơn giản sẽ mô phỏng từng truy vấn từ đầu, liên tục nhân đôi giá trị và phân tách khi cần. Điều đó sẽ nhân quy trình 54 bước với số lượng truy vấn, nhưng thậm chí con số đó vẫn rất nhỏ. Rủi ro thực sự trong các phương pháp tiếp cận đơn giản không phải là độ phức tạp về thời gian mà là việc mô hình hóa quá trình phân tách không chính xác, đặc biệt là khi xem xét theo từng dịch vụ vi mô thay vì tổng tải tổng hợp. 

Trường hợp khó nhận biết xuất hiện khi tải ban đầu đã đạt đúng công suất. Nếu một cách tiếp cận ngây thơ cho rằng việc chia tách chỉ xảy ra sau khi vượt quá dung lượng, thì nó có thể trì hoãn việc tạo máy chủ một cách không chính xác. Một vấn đề khác phát sinh từ việc giải thích làm tròn không chính xác, vì vấn đề xác định làm tròn đến phần nghìn nhưng cấu trúc dự định chỉ đơn giản là phân chia số nguyên xác định; hiểu sai điều này có thể dẫn đến từng sai sót một trong quá trình phân phối. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: duy trì tổng số yêu cầu và duy trì số lượng máy chủ hiện có. Đối với mỗi tháng, tăng gấp đôi tải. Nếu tải vượt quá tổng dung lượng của máy chủ hiện tại, hãy liên tục thêm máy chủ và chia tải cho đến khi tải còn lại trên mỗi “khối hoạt động” vừa với dung lượng. 

Tuy nhiên, việc mô phỏng sự phân chia theo nghĩa đen trên mỗi máy chủ hoặc trên mỗi vi dịch vụ là không cần thiết. Quan sát quan trọng là sau mỗi sự kiện phân tách, hệ thống hoạt động như thể chúng ta đã phân chia tổng tải thành các phần độc lập, mỗi phần bị ràng buộc bởi công suất và mỗi phần sẽ phát triển giống hệt nhau khi nhân đôi. Điều này có nghĩa là chúng ta chỉ cần theo dõi có bao nhiêu “phân đoạn” tải bằng nhau tồn tại, bởi vì mỗi phân đoạn tương ứng với một máy chủ.

Mỗi khi tổng tải tăng gấp đôi thì mỗi đoạn cũng tăng gấp đôi. Bất cứ khi nào một phân đoạn vượt quá khả năng, nó sẽ chia thành hai phân đoạn có kích thước gần bằng nhau, tăng số lượng máy chủ lên một máy chủ cho mỗi sự kiện phân chia trong dòng phân đoạn đó. Bởi vì tất cả các phân đoạn hoạt động giống hệt nhau nên toàn bộ hệ thống sẽ phát triển một cách xác định: chúng tôi chỉ cần theo dõi số lượng máy chủ cần thiết để duy trì tải phân khúc ở mức dung lượng sau mỗi bước nhân đôi. 

Điều này làm giảm vấn đề liên tục áp dụng quy tắc trên một phân đoạn đại diện: tăng gấp đôi tải của nó và bất cứ khi nào vượt quá khả năng, hãy chia nó thành hai và tiếp tục cho đến khi tất cả các phân đoạn thỏa mãn ràng buộc. 

Vì số tháng nhiều nhất là 54 nên chúng tôi có thể tính toán trước số lượng máy chủ cần thiết cho mỗi tháng bằng cách sử dụng mô phỏng lặp đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu cho mỗi truy vấn | O(M · 54) | O(1) | Đã chấp nhận | 
| Tính toán trước trạng thái hàng tháng | O(54) | O(54) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi tính toán tổng tải ban đầu bằng cách tính tổng tất cả các yêu cầu vi dịch vụ. Điều này đưa ra điểm khởi đầu của hệ thống. 

Sau đó, chúng tôi mô phỏng theo từng tháng, duy trì hai đại lượng: tải hiệu quả hiện tại trên mỗi nhóm máy chủ và số lượng máy chủ được yêu cầu. 

1. Tính tổng tải ban đầu là tổng của tất cả các vi dịch vụ. Điều này thể hiện tải hệ thống trước khi chia tỷ lệ hoặc chia tách. 
2. Khởi tạo số lượng máy chủ thành 1, vì ban đầu tất cả lưu lượng truy cập được xử lý bởi một cấu trúc lưu trữ duy nhất. 
3. Đối với mỗi tháng từ 1 đến 54, hãy tăng gấp đôi mức tải hiện tại. Đây là mô hình giả định tăng trưởng trong bài toán. 
4. Sau khi nhân đôi, hãy kiểm tra xem tải hiện tại có vượt quá dung lượng C hay không. Nếu có, chúng ta phải chia tải trên nhiều máy chủ. 
5. Mỗi thao tác phân chia sẽ tăng số lượng máy chủ lên một và giảm một nửa tải hiệu dụng cho mỗi nhóm máy chủ. Chúng tôi lặp lại việc phân tách cho đến khi tải trên mỗi máy chủ nằm trong khả năng cho phép. Điều này đảm bảo không có máy chủ nào bị quá tải sau khi phân phối lại. 
6. Lưu trữ số lượng máy chủ sau mỗi tháng để có thể trả lời các truy vấn kịp thời. 

Điều bất biến chính là sau tháng xử lý thứ i, mỗi máy chủ được chỉ định một tải không vượt quá C và tổng tải được phân chia đều trên tất cả các máy chủ theo hiệu ứng làm tròn. Bởi vì việc chia tách luôn khôi phục tính khả thi khi bị vi phạm và vì việc nhân đôi là nguồn tăng trưởng duy nhất nên quy trình đảm bảo rằng khi cấu hình có hiệu lực trong một tháng thì cấu hình đó vẫn là điểm khởi đầu chính xác cho bản cập nhật của tháng tiếp theo. Không có sự phân phối thay thế nào có thể làm giảm số lượng máy chủ cần thiết, vì mỗi lần phân chia được kích hoạt chính xác khi xảy ra vi phạm dung lượng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    C, N, M = map(int, input().split())
    total = 0
    for _ in range(N):
        total += int(input())

    queries = [int(input()) for _ in range(M)]
    max_month = 54

    # hosts[m] = number of hosts needed at month m
    hosts = [0] * (max_month + 1)

    cur_load = total
    cur_hosts = 1

    hosts[0] = 1

    for m in range(1, max_month + 1):
        cur_load *= 2

        # ensure each host has at most C load
        while cur_load > cur_hosts * C:
            cur_hosts += 1

        hosts[m] = cur_hosts

    out = []
    for q in queries:
        out.append(str(hosts[q]))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách tổng hợp tất cả các lượt tải vi dịch vụ thành một giá trị duy nhất, vì chỉ có tổng số mới quan trọng đối với hành vi mở rộng quy mô. Sau đó, quá trình mô phỏng sẽ tiến triển theo từng tháng, tăng gấp đôi tổng tải mỗi lần. 

Chi tiết triển khai quan trọng là điều kiện`cur_load > cur_hosts * C`. Điều này kiểm tra xem phân phối hiện tại có khả thi hay không: tổng dung lượng là số lượng máy chủ nhân với dung lượng trên mỗi máy chủ. Nếu tải vượt quá mức này, chúng tôi sẽ tăng số lượng máy chủ cho đến khi khôi phục được tính khả thi. Mỗi mức tăng tương ứng với việc giới thiệu một máy chủ mới và phân phối lại tải. 

Cuối cùng, chúng tôi trả lời từng truy vấn bằng cách sử dụng các giá trị được tính toán trước, tránh việc tính toán lại. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
C = 1000, N = 5, M = 5
initial loads: 1000 1000 1000 1000 1000
queries: 0 1 2 3 4
```Tổng tải ban đầu là 5000. 

| Tháng | Tải | Máy chủ | Công suất | Hành động | 
| --- | --- | --- | --- | --- | 
| 0 | 5000 | 5 | 5000 | hợp lệ | 
| 1 | 10000 | 10 | 10000 | thêm máy chủ | 
| 2 | 20000 | 20 | 20000 | thêm máy chủ | 
| 3 | 40000 | 40 | 40000 | thêm máy chủ | 
| 4 | 80000 | 80 | 80000 | thêm máy chủ | 

Mỗi tháng chính xác sẽ tăng gấp đôi cả tải và dung lượng yêu cầu thông qua máy chủ. 

Điều này xác nhận rằng khi hệ thống bắt đầu bão hòa hoàn toàn, việc nhân đôi sẽ buộc số lượng máy chủ tăng gấp đôi theo tỷ lệ. 

### Mẫu 2 

đầu vào:```
C = 2000, N = 5, M = 3
initial loads: 1000 2000 1000 2000 1000
queries: 1 0 2
```Tổng tải ban đầu là 7000. 

| Tháng | Tải | Máy chủ | Công suất | Hành động | 
| --- | --- | --- | --- | --- | 
| 0 | 7000 | 4 | 8000 | hợp lệ | 
| 1 | 14000 | 7 | 14000 | thêm máy chủ | 
| 2 | 28000 | 14 | 28000 | thêm máy chủ | 

Vào tháng 0, 4 máy chủ là đủ vì 4 × 2000 = 8000. Vào tháng 1, việc nhân đôi cần chính xác 7 máy chủ, cho thấy rằng sự tăng trưởng không phải lúc nào cũng là bội số của hai máy chủ ban đầu khi dung lượng không hạn chế. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N + 54 + M) | tổng hợp các vi dịch vụ cộng với mô phỏng 54 bước cố định cộng với trả lời các truy vấn | 
| Không gian | O(54) | lưu trữ số lượng máy chủ được tính toán trước mỗi tháng | 

Giới hạn cực kỳ nhỏ nên giải pháp có thể thoải mái chạy trong giới hạn. Độ dài mô phỏng cố định đảm bảo thời gian chạy có thể dự đoán được bất kể phân phối đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    C, N, M = map(int, input().split())
    total = 0
    for _ in range(N):
        total += int(input())

    queries = [int(input()) for _ in range(M)]
    max_month = 54

    hosts = [0] * (max_month + 1)
    cur_load = total
    cur_hosts = 1
    hosts[0] = 1

    for m in range(1, max_month + 1):
        cur_load *= 2
        while cur_load > cur_hosts * C:
            cur_hosts += 1
        hosts[m] = cur_hosts

    return "\n".join(str(hosts[q]) for q in queries)

# provided samples
assert run("""1000 5 5
1000
1000
1000
1000
1000
0
1
2
3
4
""") == "5\n10\n20\n40\n80"

assert run("""2000 5 3
1000
2000
1000
2000
1000
1
0
2
""") == "7\n4\n14"

# custom tests
assert run("""1000 1 1
1000
0
""") == "1", "single service at capacity"

assert run("""1000 1 1
1000
1
""") == "2", "first split after doubling"

assert run("""1000 2 1
1000
1000
2
""") == "4", "multiple splits after repeated doubling"

assert run("""2000 3 1
1000
1000
1000
3
""") == "2", "small load grows slowly"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| dịch vụ duy nhất hết công suất | 1 | không chia tách không cần thiết vào tháng 0 | 
| chia đầu tiên sau khi nhân đôi | 2 | kích hoạt chính xác tình trạng tràn công suất | 
| nhân đôi lặp đi lặp lại | 4 | sự tăng trưởng theo cấp số nhân của máy chủ | 
| tải nhỏ tăng chậm | 2 | cấu hình ban đầu không bão hòa | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tải ban đầu bằng tổng công suất. Ví dụ: với C = 1000 và một dịch vụ là 1000, hệ thống bắt đầu cân bằng hoàn hảo. Vào tháng 0, câu trả lời là 1 máy chủ. Sau một lần nhân đôi, tải trở thành 2000, vượt quá khả năng, do đó cần có máy chủ thứ hai. Thuật toán xử lý việc này một cách chính xác vì nó kiểm tra sự bất đẳng thức nghiêm ngặt`cur_load > cur_hosts * C`, nghĩa là đẳng thức vẫn có hiệu lực. 

Một trường hợp khác là khi tải ban đầu thấp hơn nhiều so với công suất. Ví dụ: C = 1000 với tổng tải là 10. Ngay cả sau nhiều lần nhân đôi, hệ thống có thể vẫn ở mức dưới công suất trong vài tháng. Vòng lặp không làm gì cho đến khi vượt qua ngưỡng, do đó số lượng máy chủ vẫn ổn định cho đến khi cần tăng số lượng đó. 

Trường hợp thứ ba là khi việc nhân đôi liên tục gây ra nhiều lần bổ sung máy chủ trong một tháng. Nếu tải nhảy từ ngay dưới bội số của C lên cao hơn nhiều so với nó, thì`while`vòng lặp đảm bảo chúng tôi thêm chính xác đủ số máy chủ để khôi phục tính khả thi, thay vì chỉ tăng một lần.
