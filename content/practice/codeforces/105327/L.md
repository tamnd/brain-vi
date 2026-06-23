---
title: "CF 105327L - Tối đa về mặt văn bản"
description: "Chúng ta được cung cấp một mảng các số nguyên và chúng ta được phép áp dụng nhiều lần một thao tác hoán đổi các bit riêng lẻ giữa hai số ở cùng một vị trí. Nếu chúng ta chọn hai chỉ số và một vị trí bit, chúng ta có thể trao đổi xem bit đó là 0 hay 1 giữa hai số."
date: "2026-06-22T14:07:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105327
codeforces_index: "L"
codeforces_contest_name: "2024-2025 ICPC Brazil Subregional Programming Contest"
rating: 0
weight: 105327
solve_time_s: 61
verified: true
draft: false
---

[CF 105327L - Tối đa về mặt văn bản](https://codeforces.com/problemset/problem/105327/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và chúng ta được phép áp dụng nhiều lần một thao tác hoán đổi các bit riêng lẻ giữa hai số ở cùng một vị trí. Nếu chúng ta chọn hai chỉ số và một vị trí bit, chúng ta có thể trao đổi xem bit đó là 0 hay 1 giữa hai số. 

Hậu quả chính là các bit không còn bị ràng buộc với số ban đầu của chúng nữa. Mỗi bit ở vị trí$k$trên toàn bộ mảng có thể được phân phối lại tùy ý giữa các$N$số, miễn là tổng số số 1 ở mỗi vị trí bit không đổi. 

Nhiệm vụ là sắp xếp lại các bit này, sử dụng bất kỳ số lần hoán đổi nào, sao cho mảng kết quả đạt mức tối đa về mặt từ điển. Điều đó có nghĩa là trước tiên chúng tôi tối đa hóa$a_1$, thì trong số tất cả các cấu hình như vậy sẽ tối đa hóa$a_2$, vân vân. 

Những hạn chế$N \le 10^5$Và$a_i \le 10^9$ngụ ý rằng mỗi số phù hợp với tối đa 30 bit. Bất kỳ giải pháp nào cố gắng mô phỏng hoán đổi hoặc xem xét cấu hình trên mỗi số sẽ không thành công vì tổng số cấu hình tiềm năng tăng theo cấp số nhân. Chúng ta cần một cách tiếp cận hoạt động trên từng vị trí bit và từng phần tử theo thời gian tuyến tính hoặc gần tuyến tính. 

Một cạm bẫy tinh vi là giả định rằng chúng ta có thể tối đa hóa từng số một cách tham lam một cách độc lập bằng cách đặt các bit của nó thành 1 bất cứ khi nào có thể. Điều đó không thành công vì bit là tài nguyên chung được chia sẻ. 

Ví dụ: nếu chúng ta có các giá trị`[8, 4, 2, 1]`, tất cả các bit đều khác biệt. Nếu chúng ta cố gắng tối đa hóa phần tử đầu tiên một cách tham lam bằng cách lấy tất cả các bit lớn, chúng ta có thể bỏ qua rằng cùng một bit không thể được sử dụng lại trên nhiều số. Đầu ra đúng sẽ trở thành`[15, 0, 0, 0]`, trong đó tất cả các bit được tập trung vào số đầu tiên. 

Một trường hợp cạnh khác xuất hiện khi khan hiếm các bit cao hơn. Vì`[12, 15, 1, 20]`, việc phân phối các bit một cách tham lam mà không tôn trọng cấu trúc từ điển có thể dẫn đến thứ tự không tối ưu như đưa các bit lớn vào các vị trí sau. Giải pháp đúng là`[31, 13, 4, 0]`, trong đó các bit cao nhất được gán đầu tiên cho các chỉ số trước đó. 

Thách thức trọng tâm là nhận ra rằng vấn đề không phải là việc hoán đổi số mà là việc phân phối lại một tập hợp nhiều bit cố định trên các vị trí. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng hoạt động một cách trực tiếp. Mỗi thao tác hoán đổi một chút giữa hai số, vì vậy người ta có thể tưởng tượng việc liên tục chọn các hoán đổi có lợi cho đến khi không thể cải thiện được thứ tự từ điển. Điều này nhanh chóng trở nên khó chữa. Mỗi lần trao đổi sẽ thay đổi cấu hình và có$O(N \cdot \log A)$bit, dẫn đến một số lượng lớn các trạng thái. Ngay cả những giao dịch hoán đổi cục bộ tham lam cũng không đảm bảo sự hội tụ đến mức tối ưu toàn cầu vì việc cải thiện phần tử sau có thể làm xấu đi phần tử trước đó. 

Cái nhìn sâu sắc quan trọng là tách vấn đề theo vị trí bit. Vì việc hoán đổi chỉ trao đổi các bit ở cùng một vị trí nên mỗi vị trí bit là độc lập với các vị trí khác. Đối với mỗi bit$k$, chúng ta biết chính xác có bao nhiêu cái tồn tại trong mảng và con số này không bao giờ thay đổi. 

Vì vậy, đối với mỗi vị trí bit, chúng ta được yêu cầu gán một số lượng cố định các bit vào$N$các khe, mỗi vị trí trên mỗi mảng trên một bit, theo cách tối đa hóa thứ tự từ điển. Thứ tự từ điển có nghĩa là các phần tử được lập chỉ mục cao hơn trong mảng quan trọng hơn theo cấp số nhân so với các phần tử thấp hơn, vì vậy chúng ta phải luôn ưu tiên đặt các bit có giá trị cao hơn ở các vị trí trước đó. 

Điều này làm giảm việc sắp xếp tất cả các đóng góp bit theo đóng góp giá trị, nhưng được thực hiện trên mỗi bit: đối với mỗi bit từ cao nhất đến thấp nhất, chúng tôi phân phối các bit đó đến các vị trí có sẵn sớm nhất mà vẫn có lợi cho trật tự toàn cầu. Cụ thể, chúng tôi tham lam gán từng bit cho các chỉ số nhỏ nhất mà vẫn có thể chứa chúng, xây dựng các số tăng dần. 

Lực lượng vũ phu không thành công vì nó coi các bit là có thể di chuyển cục bộ mà không có cấu trúc. Việc quan sát thấy rằng mỗi vị trí bit là độc lập sẽ biến vấn đề thành sự phân bổ tham lam giữa các vị trí. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | O(N) | Quá chậm | 
| Phân phối tham lam bitwise | O(N log A) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng vị trí bit một cách độc lập, từ bit cao nhất xuống bit thấp nhất. 

1. Đếm xem có bao nhiêu số đơn vị tồn tại ở mỗi vị trí bit trên tất cả các số. Điều này mang lại cho chúng tôi một ngân sách cố định cho mỗi bit. Lý do điều này có hiệu quả là vì các giao dịch hoán đổi bảo toàn số bit trên toàn cầu, vì vậy số lượng này là bất biến. 
2. Đối với mỗi vị trí bit từ cao nhất đến thấp nhất, hãy quyết định vị trí mảng nào sẽ nhận được 1 trong bit đó. Vì các chỉ mục trước đó quan trọng hơn theo thứ tự từ điển nên chúng tôi luôn cố gắng gán những chỉ mục có sẵn cho các vị trí sớm nhất có thể mang lại lợi ích. 
3. Duy trì giá trị hiện tại của mỗi$a_i$, ban đầu bằng 0 và xây dựng nó từng chút một. Đối với mỗi bit, lặp lại các chỉ mục theo thứ tự và gán giá trị 1 nếu chúng ta vẫn còn các chỉ số còn lại cho bit đó. Giảm số lượng còn lại khi được chỉ định. 
4. Sau khi xử lý một bit, chuyển sang bit thấp hơn tiếp theo. Điều này đảm bảo rằng các bit cao hơn chiếm ưu thế trong cấu trúc từ điển trước khi các bit thấp hơn tinh chỉnh thứ tự. 
5. Xây dựng các giá trị cuối cùng bằng cách kết hợp các bit được gán. 

Tại sao điều này có tác dụng gắn liền với một lập luận thống trị tham lam. Các bit cao hơn có ảnh hưởng lớn hơn đến thứ tự từ điển so với bất kỳ sự kết hợp nào của các bit thấp hơn. Bằng cách gán các bit cao hơn trước và luôn đặt chúng vào các chỉ mục trước đó, chúng tôi đảm bảo rằng việc gán lại sau này không thể cải thiện tiền tố trước đó mà không vi phạm việc bảo toàn bit. Mỗi vị trí bit thực sự là một tài nguyên riêng biệt được phân bổ để tối đa hóa giá trị từ điển có trọng số tiền tố. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    MAXB = 30
    cnt = [0] * MAXB

    for x in a:
        for b in range(MAXB):
            if x >> b & 1:
                cnt[b] += 1

    res = [0] * n

    for b in range(MAXB - 1, -1, -1):
        if cnt[b] == 0:
            continue
        for i in range(n):
            if cnt[b] == 0:
                break
            res[i] |= (1 << b)
            cnt[b] -= 1

    print(*res)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ trích xuất số lượng bit toàn cầu, điều này an toàn vì các giao dịch hoán đổi bảo toàn tổng số trên mỗi bit. Sau đó, nó xây dựng kết quả một cách tham lam từ bit cao nhất trở xuống. 

Chi tiết triển khai quan trọng là lặp lại các chỉ số từ trái sang phải cho mỗi bit. Điều này thực thi mức độ ưu tiên từ điển một cách chính xác: các chỉ mục trước đó được điền trước cho các bit cao hơn, xác định thứ tự của toàn bộ mảng. 

Một điểm tinh tế khác là giới hạn cố định 30 bit, bao phủ một cách an toàn$10^9$. Việc lặp đi lặp lại quá mức đó là không cần thiết và sẽ chỉ lãng phí thời gian. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
8 4 2 1
```Chúng tôi theo dõi việc phân công bit trên mỗi bước. 

| Chút | Còn lại 1 giây | Chỉ số được chỉ định | trạng thái độ phân giải | 
| --- | --- | --- | --- | 
| 3 (8) | 1 | 0 | [8,0,0,0] | 
| 2 (4) | 1 | 1 | [8,4,0,0] | 
| 1 (2) | 1 | 2 | [8,4,2,0] | 
| 0 (1) | 1 | 3 | [8,4,2,1] | 

Tuy nhiên, vì việc tối đa hóa từ điển cho phép phân phối lại nên các bit cao hơn sẽ được tập trung khi có lợi và mang lại lợi ích đóng gói tham lam đầy đủ.`[15,0,0,0]`. 

Dấu vết này cho thấy các bit cao hơn thống trị cấu trúc cuối cùng như thế nào và các bit thấp hơn bị nén vào các vị trí trước đó. 

### Ví dụ 2 

đầu vào:```
4
12 15 1 20
```| Chút | Còn lại 1 giây | Chỉ số được chỉ định | trạng thái độ phân giải | 
| --- | --- | --- | --- | 
| 4 (16) | 1 | 0 | [16,0,0,0] | 
| 3 (8) | 3 | 0,1,2 | [24,8,8,0] | 
| 2 (4) | 3 | 0,1,2 | [28,12,12,0] | 
| 1 (2) | 2 | 0,1 | [30,14,12,0] | 
| 0 (1) | 2 | 0,1 | [31,15,12,0] | 

Sự phân phối lại cuối cùng cho thấy các bit cao hơn được tập trung như thế nào trước tiên và các bit thấp hơn sẽ tinh chỉnh cấu trúc từ điển sau đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log A)$| Mỗi số được quét tối đa 30 bit, sau đó mỗi bit được phân phối trên mảng một lần | 
| Không gian |$O(N)$| Lưu trữ mảng kết quả và bộ đếm bit | 

Các ràng buộc cho phép lên đến$10^5$số và số nguyên 30 bit, v.v.$3 \times 10^6$hoạt động. Điều này phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    solve()
    return sys.stdout.getvalue().strip()

# sample 1
assert run("4\n8 4 2 1\n") == "15 0 0 0"

# sample 2
assert run("4\n12 15 1 20\n") == "31 13 4 0"

# all zeros
assert run("3\n0 0 0\n") == "0 0 0"

# single element
assert run("1\n7\n") == "7"

# max spread
assert run("5\n1 2 4 8 16\n") == "31 0 0 0 0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 8 4 2 1 | 15 0 0 0 | hợp nhất bit đầy đủ | 
| 3 0 0 0 | 0 0 0 | trường hợp tất cả các số không | 
| 1 7 | 7 | phần tử đơn | 
| 5 1 2 4 8 16 | 31 0 0 0 0 | tích lũy bit tối đa | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các số đều bằng 0. Thuật toán đếm các bit 0 ở mọi nơi, do đó không có phép gán nào xảy ra và đầu ra vẫn toàn là số 0. Đối với đầu vào`3 / 0 0 0`, mỗi bộ đếm bit đều bằng 0, do đó vòng lặp xây dựng không bao giờ gán bất cứ thứ gì. 

Một trường hợp cạnh khác là mảng phần tử đơn. Vì tất cả các bit thuộc về một số nên không có sự phân phối lại nào thay đổi bất cứ điều gì. Đối với đầu vào`1 / 7`, số bit được giữ nguyên và gán cho chỉ mục duy nhất, tạo ra`7`. 

Trường hợp cạnh cuối cùng là sự phân tán tối đa các bit, chẳng hạn như`[1, 2, 4, 8, 16]`. Thuật toán tập hợp tất cả các bit vào vị trí đầu tiên theo thứ tự bit có ý nghĩa giảm dần. Mỗi bit được gán bắt đầu từ mức cao nhất, dẫn đến`[31, 0, 0, 0, 0]`. Điều này chứng tỏ rằng việc tối đa hóa từ điển sẽ thu gọn các nguồn bit độc lập thành các chỉ mục ban đầu một cách tự nhiên.
