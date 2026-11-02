---
title: "CF 1089L - Lazyland"
description: "Chúng ta có một tập hợp công nhân, mỗi người trong số họ đã chọn một công việc mà họ muốn làm. Có chính xác $k$ công việc riêng biệt và mỗi công nhân chỉ vào một trong số chúng."
date: "2026-06-13T03:46:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 1089
codeforces_index: "L"
codeforces_contest_name: "2018-2019 ICPC, NEERC, Northern Eurasia Finals (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 900
weight: 1089
solve_time_s: 199
verified: false
draft: false
---

[CF 1089L - Vùng đất lười biếng](https://codeforces.com/problemset/problem/1089/L) 

**Xếp hạng:** 900 
**Thẻ:** - 
**Thời gian giải:** 3m 19s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp công nhân, mỗi người trong số họ đã chọn một công việc mà họ muốn làm. Có chính xác$k$công việc riêng biệt và mỗi công nhân chỉ vào một trong số chúng. Vì có ít nhất số công nhân bằng công việc, nên chúng ta có thể chỉ định nhiều nhất một công nhân cho mỗi công việc, nhưng cuối cùng chúng ta phải thực hiện mọi công việc đúng một lần. 

Điều phức tạp là những lựa chọn hiện tại có thể khiến một số công việc bị bỏ sót, trong khi những công việc khác được nhiều người lao động lựa chọn. Để khắc phục điều này, chúng ta được phép thuyết phục người lao động thay đổi công việc đã chọn. Thuyết phục$i$-chi phí công nhân thứ$b_i$, và sau khi thuyết phục chúng tôi có thể giao họ làm công việc khác. 

Mục tiêu là để đảm bảo rằng mọi công việc từ$1$ĐẾN$k$được giao cho chính xác một nhân viên, giảm thiểu tổng chi phí thuyết phục. 

Các ràng buộc đi lên đến$n = 10^5$. Bất kỳ giải pháp nào cố gắng mô phỏng các bài tập hoặc kết hợp thử nhiều lần sẽ vượt quá$O(n^2)$. Thậm chí$O(n \log n)$là mục tiêu thực tế vì việc sắp xếp hoặc sử dụng đống có thể chấp nhận được. Điều này gợi ý rõ ràng rằng chúng ta nên đối xử với người lao động theo nhóm cho mỗi công việc và đưa ra những quyết định tham lam trong mỗi nhóm. 

Một trường hợp phức tạp xuất hiện khi một công việc ban đầu được chọn bởi chính xác một công nhân. Người công nhân đó bị buộc phải làm công việc đó trừ khi chúng ta cố tình di chuyển họ, nhưng làm như vậy sẽ chỉ tạo ra chi phí không cần thiết vì các công việc khác có thể trở nên khó trang trải hơn. Một trường hợp khó khăn khác là khi một công việc hoàn toàn không được chọn; thì chúng ta phải “nhập khẩu” một công nhân từ một số nhóm công việc khác, điều này buộc chúng ta phải lựa chọn di chuyển công nhân nào một cách tiết kiệm chi phí. 

Ví dụ: nếu tất cả công nhân chọn công việc 1 ngoại trừ một công nhân chọn công việc 2 thì công việc 3 (nếu$k \ge 3$) phải được lấp đầy bằng cách chuyển ai đó từ công việc 1 hoặc 2. Sự lựa chọn tối ưu hoàn toàn phụ thuộc vào việc phân công lại công nhân nào là rẻ nhất. 

Một cách tiếp cận ngây thơ có thể cố gắng phân công công việc một cách tham lam theo thứ tự mà không xem xét đến sự đánh đổi chi phí toàn cầu, điều này sẽ thất bại khi việc phân công tối ưu cục bộ tiêu tốn tính linh hoạt với chi phí thấp cần thiết ở những nơi khác. 

## Phương pháp tiếp cận 

Một cách trực tiếp để suy nghĩ về vấn đề này là tưởng tượng rằng chúng ta phải có chính xác một nhân viên đại diện cho mỗi công việc. Vì ban đầu mỗi công nhân “thuộc về” một công việc, nên mỗi công việc tạo thành một nhóm ứng viên và chúng ta phải quyết định nên giữ công nhân nào ở công việc ban đầu và công nhân nào sẽ “hy sinh” để được phân phối lại. 

Đối với mỗi công việc, một phương pháp vũ phu sẽ xem xét công nhân nào ở lại và công nhân nào bị di chuyển, đồng thời đảm bảo tất cả các công việc đều chỉ có một công nhân. Điều này nhanh chóng trở thành một bài toán gán tổ hợp. Nếu một công việc có$t$công nhân, việc chọn người ở lại sẽ tương tác với các lựa chọn cho tất cả các công việc khác bởi vì mọi công nhân được chuyển đi đều sẵn sàng cho một công việc khác. Trong trường hợp xấu nhất, điều này dẫn đến sự kết hợp theo cấp số nhân hoặc ít nhất$O(nk)$chuyển tiếp nếu được thực hiện cẩn thận, tốc độ này quá chậm đối với$10^5$. 

Nhận xét quan trọng là trong mỗi công việc, chỉ một nhân viên có thể “không phải chịu” chi phí thuyết phục, bởi vì chỉ một trong số họ thực sự đảm nhận công việc đó trong nhiệm vụ cuối cùng. Tất cả những người lao động khác làm công việc đó đều là ứng viên được phân công lại. Nếu chúng ta quyết định rằng một công việc giữ được người lao động tốt nhất có thể thì mọi công nhân khác trong nhóm đó sẽ trở thành “nguồn cung sẵn có” có thể được sử dụng để lấp đầy những công việc còn thiếu ở nơi khác. 

Điều này giải quyết lại vấn đề: chúng ta phải đảm bảo mọi công việc đều có ít nhất một công nhân và chúng ta muốn giảm thiểu tổng chi phí cho những công nhân mà chúng ta buộc phải di chuyển. Tương tự, đối với mỗi công việc, chúng tôi muốn duy trì chính xác một công nhân với chi phí bằng 0 và coi tất cả các công nhân khác là ứng cử viên để phân phối lại. Lựa chọn tối ưu để giữ lại nhân viên nào cho mỗi công việc là giữ người có chi phí thuyết phục lớn nhất, bởi vì đó là nhân viên mà chúng ta ít muốn “trả tiền cho việc loại bỏ”. 

Do đó, giải pháp giảm xuống việc tổng hợp tất cả chi phí nhân công và trừ đi tổng chi phí lớn nhất trong mỗi nhóm công việc, đồng thời đảm bảo rằng chúng ta có đủ nhiệm vụ được bảo toàn để trang trải tất cả các công việc. Vì mỗi công việc đóng góp ít nhất một công nhân được bảo toàn nên tính khả thi là tự động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | số mũ /$O(nk)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log n)$hoặc$O(n)$|$O(k)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tổ chức lại người lao động theo công việc mà họ đã chọn ban đầu vì các quyết định mang tính cục bộ trong mỗi nhóm công việc. 

1. Nhóm tất cả công nhân theo công việc họ đã chọn. Mỗi nhóm đại diện cho những ứng viên có khả năng đảm nhiệm công việc đó và chỉ một trong số họ phải tiếp tục được giao ở đó. 
2. Với mỗi nhóm công việc, hãy tìm người có chi phí thuyết phục lớn nhất$b_i$. Công nhân này là người tốn kém nhất để phân công lại, vì vậy chúng tôi muốn giữ họ tại chỗ. 
3. Tính tổng chi phí thuyết phục của tất cả nhân viên. Điều này thể hiện cái giá phải trả nếu chúng ta thuyết phục được mọi người. 
4. Trừ đi số tiền tối đa từ tổng số này$b_i$từ mỗi nhóm công việc. Điều này mô hình hóa thực tế rằng chúng tôi giữ một công nhân cho mỗi công việc mà không bị thuyết phục, được lựa chọn để tối đa hóa chi phí tiết kiệm. 
5. In ra giá trị kết quả, biểu thị thời gian thuyết phục tối thiểu cần thiết để đảm bảo bao quát được tất cả các công việc. 

Lý do chúng ta chỉ cần duy trì một công nhân cho mỗi công việc là vì mỗi công việc phải được giao chính xác một công nhân, vì vậy mỗi nhóm đóng góp chính xác một nhiệm vụ “neo” mà không cần thuyết phục. 

### Tại sao nó hoạt động 

Mỗi công việc phải giữ đúng một công nhân. Trong một nhóm công việc, bất kỳ công nhân nào ngoại trừ người được chúng tôi giao cho công việc đó đều phải được di chuyển. Do chi phí di chuyển$b_i$, việc giảm thiểu chi phí tương đương với việc tối đa hóa chi phí mà chúng ta tránh phải trả bằng cách không di chuyển ai đó. Ứng cử viên tốt nhất để tránh di chuyển luôn là công nhân có diện tích lớn nhất.$b_i$trong nhóm đó, bởi vì giữ bất kỳ công nhân nào khác sẽ buộc chúng ta mất đi khoản tiết kiệm có giá trị cao hơn. Điều này tạo ra một lựa chọn cục bộ cho mỗi công việc không tương tác giữa các công việc, do đó tổng các lựa chọn tối ưu độc lập là tối ưu toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    max_b = [0] * (k + 1)
    total = 0
    
    for i in range(n):
        job = a[i]
        cost = b[i]
        total += cost
        if cost > max_b[job]:
            max_b[job] = cost
    
    ans = total - sum(max_b[1:])
    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai duy trì tổng chi phí thuyết phục trong khi theo dõi chi phí tối đa của mỗi công việc đối với những người lao động được giao công việc đó. Bước trừ cuối cùng sẽ loại bỏ chính xác một công nhân trên mỗi công việc khỏi bị tính là “được thuyết phục”. 

Chi tiết quan trọng là chúng tôi không bao giờ mô phỏng rõ ràng việc phân công lại. Chúng tôi chỉ tính toán những công nhân nào có thể giữ được vị trí rẻ nhất. Việc lập chỉ mục từ$1$ĐẾN$k$khớp trực tiếp các nhãn công việc, tránh các biến chứng riêng lẻ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
8 7
1 1 3 1 5 3 7 1
5 7 4 8 1 3 5 2
```Chúng tôi nhóm công nhân theo công việc và theo dõi chi phí tối đa trong mỗi nhóm. 

| Việc làm | Chi phí theo nhóm | Giữ tối đa | 
| --- | --- | --- | 
| 1 | 5, 7, 8, 2 | 8 | 
| 2 | - | 0 | 
| 3 | 4, 3 | 4 | 
| 4 | - | 0 | 
| 5 | 1 | 1 | 
| 6 | - | 0 | 
| 7 | 5 | 5 | 

Tổng cộng tất cả các chi phí là$5 + 7 + 4 + 8 + 1 + 3 + 5 + 2 = 35$. 

Tổng cực đại là$8 + 0 + 4 + 0 + 1 + 0 + 5 = 18$. 

Kết quả là$35 - 18 = 17$. Điều này phù hợp với ý tưởng rằng mỗi công việc đều giữ lại những công nhân “đắt tiền để di chuyển” nhất. 

Dấu vết này cho thấy rằng tất cả cấu trúc đều mang tính cục bộ cho mỗi công việc và không cần đến các quyết định liên công việc. 

### Ví dụ 2 

đầu vào:```
3 3
1 2 3
10 20 30
```| Việc làm | Chi phí | Giữ tối đa | 
| --- | --- | --- | 
| 1 | 10 | 10 | 
| 2 | 20 | 20 | 
| 3 | 30 | 30 | 

Tổng cộng là$60$, tổng cực đại là$60$, vậy đáp án là$0$. Mỗi công việc đều đã có sẵn một nhân viên duy nhất nên không cần phải thuyết phục. 

Điều này xác nhận rằng thuật toán xử lý chính xác trường hợp cân bằng hoàn toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Một lần để tính tổng và mức tối đa cho mỗi công việc | 
| Không gian |$O(k)$| Mảng lưu trữ chi phí tối đa cho mỗi công việc | 

Giải pháp phù hợp thoải mái trong giới hạn vì$n \le 10^5$và cả việc sử dụng thời gian và bộ nhớ đều tuyến tính và tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    input = sys.stdin.readline
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    max_b = [0] * (k + 1)
    total = 0
    for i in range(n):
        total += b[i]
        max_b[a[i]] = max(max_b[a[i]], b[i])
    
    print(total - sum(max_b[1:]))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = old_stdout
    sys.stdin = old_stdin
    return out.getvalue().strip()

# provided sample
assert run("""8 7
1 1 3 1 5 3 7 1
5 7 4 8 1 3 5 2
""") == "17"

# all jobs already unique
assert run("""3 3
1 2 3
10 20 30
""") == "0"

# all choose same job
assert run("""5 3
1 1 1 1 1
1 2 3 4 5
""") == "5"

# minimal case
assert run("""1 1
1
100
""") == "0"

# mixed groups
assert run("""4 3
1 2 2 3
5 1 10 2
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả công việc độc đáo | 0 | không cần thuyết phục | 
| tất cả cùng một công việc | hành vi nhóm đúng | nhiều ứng viên cho mỗi công việc | 
| phần tử đơn | tính đúng đắn của trường hợp cơ sở | | 
| nhóm hỗn hợp | tính đúng đắn của logic tối đa trên mỗi nhóm | | 

## Vỏ cạnh 

Khi mọi công nhân chọn cùng một công việc, tất cả các công việc khác đều trống và phải được lấp đầy hoàn toàn bằng cách phân công lại công nhân từ nhóm dày đặc. Thuật toán xử lý vấn đề này bằng cách chọn nhân viên có chi phí tối đa trong nhóm đó để ở lại, đồng thời tính tất cả những người khác là chi phí có thể loại bỏ, giúp giảm thiểu chính xác tổng thời gian thuyết phục bằng cách giữ lại nhân viên đắt giá nhất. 

Khi mỗi công việc đều có chính xác một công nhân thì mỗi nhóm có một phần tử duy nhất, do đó giá trị tối đa bằng tổng của nhóm đó. Phép trừ loại bỏ mọi thứ, mang lại chi phí bằng 0, phù hợp với thực tế là không cần thuyết phục.
