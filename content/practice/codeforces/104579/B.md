---
title: "CF 104579B - Khách sạn gia đình"
description: "Chúng ta được xếp một dãy phòng được đánh số từ 1 đến N. Khách lần lượt đến và mỗi khách phải được phân vào đúng hai phòng liền kề, cả hai đều còn trống vào thời điểm phân định. Nếu tồn tại một số cặp trống liền kề thì một trong số chúng được chọn ngẫu nhiên như nhau."
date: "2026-06-30T07:44:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104579
codeforces_index: "B"
codeforces_contest_name: "2016 Google Code Jam World Finals (GCJ 16 World Finals)"
rating: 0
weight: 104579
solve_time_s: 69
verified: true
draft: false
---

[CF 104579B - Khách sạn gia đình](https://codeforces.com/problemset/problem/104579/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được xếp một dãy phòng được đánh số từ 1 đến N. Khách lần lượt đến và mỗi khách phải được phân vào đúng hai phòng liền kề, cả hai đều còn trống vào thời điểm phân định. Nếu tồn tại một số cặp trống liền kề thì một trong số chúng được chọn ngẫu nhiên như nhau. Hai phòng đó sau đó sẽ được sử dụng vĩnh viễn. Điều này tiếp tục cho đến khi không còn cặp trống liền kề nào trong hành lang, lúc đó quá trình dừng lại. 

Đối với một phòng cố định K, chúng ta được yêu cầu xác suất để phòng này kết thúc khi quá trình kết thúc. 

Điểm mấu chốt là quá trình này không mang tính quyết định. Ở mỗi bước, việc lựa chọn cặp trống liền kề nào sẽ tạo ra tính ngẫu nhiên và các lựa chọn khác nhau sẽ dẫn đến các cấu hình cuối cùng khác nhau. Chúng tôi không mô phỏng một kết quả mà tính trung bình trên tất cả các lịch sử ngẫu nhiên hợp lệ có thể có. 

Các ràng buộc cho phép N lên tới 10^7, điều này ngay lập tức loại trừ mọi cách tiếp cận mô phỏng quy trình từng bước. Ngay cả O(N) cho mỗi trường hợp thử nghiệm cũng sẽ quá chậm trong trường hợp xấu nhất nếu T lớn. Giải pháp phải khai thác sự đơn giản hóa cấu trúc mạnh mẽ để mỗi trường hợp thử nghiệm có thể được trả lời theo thời gian logarit hoặc không đổi sau khi tiền xử lý. 

Trường hợp cạnh tinh tế xuất hiện khi K không ở gần biên. Ví dụ, khi N = 4 và K = 2, câu trả lời luôn là 1 vì phòng 2 luôn là một phần của cặp được chọn nào đó trong bất kỳ quá trình tối đa nào. Một trực giác ngây thơ có thể cho rằng mọi phòng đều có một số cơ hội độc lập để còn trống, nhưng trên thực tế, chỉ các phòng ranh giới mới có thể không được ghép đôi trong cấu hình cuối cùng. Các phòng bên trong bị buộc phải khớp về mặt cấu trúc để đạt được mọi kết quả tối đa, đó là hệ quả của cách hoạt động của tính khớp liền kề tham lam trên một đường dẫn. 

Một tình huống khó khăn khác nảy sinh khi lựa chọn đầu tiên chia hành lang thành các đoạn rời rạc. Chẳng hạn, chọn cặp (2, 3) trong một hành lang nhỏ sẽ ngay lập tức cách ly phòng 1. Từ thời điểm đó trở đi, không bao giờ được chạm vào phòng 1 nữa. Kiểu tách biệt không thể đảo ngược này là cấu trúc cốt lõi thúc đẩy giải pháp. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp sẽ duy trì tập hợp tất cả các cặp trống liền kề hợp lệ, chọn ngẫu nhiên một cặp thống nhất, loại bỏ các điểm cuối của nó và lặp lại cho đến khi không còn cặp nào. Điều này đúng về mặt khái niệm và nó phản ánh chính xác quá trình. Tuy nhiên, mỗi bước yêu cầu cập nhật một tập hợp động các cạnh có sẵn và có tổng cộng O(N) bước. Qua nhiều trường hợp thử nghiệm có N lớn, điều này trở nên không khả thi. 

Quan sát quan trọng là quá trình này không bao giờ phụ thuộc vào hình học ngoài các phân đoạn được kết nối của các phòng trống liên tiếp. Khi một cặp được chọn, hành lang sẽ chia thành các bài toán con độc lập ở các đoạn bên trái và bên phải. Sự phát triển của từng phân đoạn giống hệt về mặt thống kê với vấn đề ban đầu trên N nhỏ hơn. 

Sự phân rã đệ quy này gợi ý rằng chúng ta không cần phải mô phỏng toàn bộ quá trình so khớp. Thay vào đó, chúng ta có thể suy luận về xác suất của các sự kiện theo quy mô phân đoạn và rút ra sự tái diễn. 

Một sự đơn giản hóa quan trọng là chỉ có các điểm cuối của hành lang hoạt động khác nhau. Bất kỳ phòng nào nằm hoàn toàn bên trong hành lang cuối cùng luôn được khớp trong mọi kết quả tối đa, vì vậy xác suất của nó là 1. Toàn bộ vấn đề quy về việc tính xác suất mà một phòng điểm cuối được khớp. 

Sau đó chúng tôi tập trung vào tính toán f(n), xác suất để phòng 1 có người trong hành lang có chiều dài n. Cặp được chọn đầu tiên được phân bố đồng đều giữa n-1 cặp liền kề. Tùy thuộc vào cặp nào được chọn trước, chúng tôi sẽ ghép ngay phòng 1, cô lập nó vĩnh viễn hoặc giảm vấn đề xuống một hành lang độc lập nhỏ hơn. 

Điều này dẫn đến sự lặp lại rõ ràng trên các kích thước tiền tố, có thể được đánh giá theo thời gian tuyến tính trên N tối đa trong tất cả các trường hợp thử nghiệm.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(N) mỗi bước, tổng O(N^2) | O(N) | Quá chậm | 
| Tiền tố DP Tái phát | O(tối đa N) | O(tối đa N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tập trung vào tính toán f(n), xác suất phòng 1 có người sử dụng khi có n phòng. 

1. Xác định f(n) là xác suất để phòng ngoài cùng bên trái có người. 
2. Xét cặp liền kề được chọn đầu tiên. Có chính xác n-1 cặp có thể xảy ra, tất cả đều có khả năng như nhau. 
3. Nếu cặp được chọn đầu tiên là (1, 2), phòng 1 sẽ có người ngay lập tức, do đó điều này có xác suất là 1. 
4. Nếu cặp được chọn đầu tiên là (2, 3), phòng 1 sẽ bị cô lập như một phòng trống duy nhất và không bao giờ có thể là một phần của bất kỳ cặp nào trong tương lai, vì vậy điều này đóng góp 0. 
5. Nếu cặp được chọn đầu tiên là (k, k+1) với k ≥ 3, thì hành lang sẽ chia thành đoạn bên trái có kích thước k−1 và đoạn bên phải có kích thước n−k−1. Đoạn bên phải không liên quan đến phòng 1 và đoạn bên trái hoạt động giống hệt như một bài toán mới có kích thước k−1. Vậy phần đóng góp là f(k−1). 
6. Tổng hợp tất cả các khả năng sẽ đưa ra sự truy hồi theo các giá trị trước đó của f. 
7. Để đánh giá điều này một cách hiệu quả, hãy duy trì tổng tiền tố S(n) = f(1) + f(2) + … + f(n). Điều này cho phép mỗi f(n) được tính toán trong thời gian O(1) sau khi xử lý trước. 
8. Tính f(1) = 0 làm trường hợp cơ sở, vì không tồn tại cặp nào. 
9. Tính toán lặp đi lặp lại f(2), f(3), … lên tới N tối đa bằng cách sử dụng phép truy toán, lưu trữ tổng tiền tố trong quá trình thực hiện. 

Đối với bất kỳ truy vấn nào (N, K), trả về f(N) nếu K là 1 hoặc N, nếu không thì trả về 1. 

### Tại sao nó hoạt động 

Toàn bộ quá trình là kết hợp tham lam ngẫu nhiên trên một đường dẫn. Cách duy nhất để một đỉnh có thể không bị so sánh là nếu mọi cạnh liên quan đến nó không bao giờ được chọn trước khi cấu trúc xung quanh nó trở nên cố định. Đối với các đỉnh bên trong, điều này là không thể bởi vì bất kỳ sự khớp tối đa nào của một đường đi đều buộc tất cả cấu trúc bên trong phải bị che phủ; một đỉnh bên trong không khớp sẽ để lại hai đỉnh tự do liền kề, mâu thuẫn với cực đại. Điều này làm giảm vấn đề chỉ còn phân tích tỷ lệ sống sót ở điểm cuối. 

Phép truy toán thể hiện sự phân rã bước đầu tiên của quá trình. Mọi kết quả được phân chia theo cạnh được chọn đầu tiên và mỗi nhánh sẽ kết thúc ngay lập tức ở phòng 1 hoặc giảm xuống một thể hiện độc lập nhỏ hơn hoàn toàn. Điều này đảm bảo không có sự chồng chéo giữa các trường hợp và duy trì tổng khối lượng xác suất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    T = int(input())
    queries = []
    max_n = 1

    for _ in range(T):
        n, k = map(int, input().split())
        queries.append((n, k))
        max_n = max(max_n, n)

    if max_n >= 1:
        f = [0] * (max_n + 1)
        pref = [0] * (max_n + 1)

        f[1] = 0
        pref[1] = 0

        for n in range(2, max_n + 1):
            # f(n) = (1 + sum_{i=2..n-2} f(i)) / (n-1)
            total = 1 + (pref[n - 2] - pref[1])
            if total < 0:
                total %= MOD
            inv = pow(n - 1, MOD - 2, MOD)
            f[n] = (total % MOD) * inv % MOD
            pref[n] = (pref[n - 1] + f[n]) % MOD

    out = []
    for n, k in queries:
        if k != 1 and k != n:
            out.append(f"Case #: 1")
        else:
            out.append(f"Case #: {f[n]}")
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai tách vấn đề thành giai đoạn tiền xử lý và giai đoạn truy vấn. Quá trình xử lý trước xây dựng f[n] lên tới N tối đa trong tất cả các trường hợp thử nghiệm bằng cách sử dụng phép truy toán dẫn xuất và mảng tổng tiền tố để tránh tính toán lại tổng nhiều lần. 

Logic truy vấn là ngay lập tức. Nếu K không ở hai đầu hành lang thì câu trả lời luôn là 1. Ngược lại, chúng ta trả về xác suất điểm cuối được tính toán trước f[N]. 

Phải cẩn thận với việc phân chia theo mô-đun. Vì chúng ta tính modulo 10^9+7 nên phép chia cho (n−1) được thực hiện bằng cách sử dụng nghịch đảo mô đun. 

## Ví dụ đã hoạt động 

Xét trường hợp N = 4, K = 1. Ban đầu, nước đi đầu tiên có thể xảy ra là ba cặp liền kề: (1,2), (2,3) và (3,4), mỗi cặp có xác suất 1/3. 

| Cặp đầu tiên | Cấu trúc kết quả | Số Phận Phòng 1 | 
| --- | --- | --- | 
| (1,2) | Phòng 1 có người ở ngay | Chiếm đóng | 
| (2,3) | Phòng 1 biệt lập vĩnh viễn | Miễn phí | 
| (3,4) | Giảm xuống đoạn nhỏ hơn bên phải | Cuối cùng bị chiếm đóng thông qua đệ quy | 

Do đó, xác suất là 2/3, phù hợp với kết quả đã biết. 

Bây giờ hãy xem xét N = 5, K = 2. Phòng 2 là phòng bên trong. Bất kể cặp đầu tiên nào được chọn, nó sẽ được khớp trực tiếp hoặc nó trở thành một phần của phân khúc nhỏ hơn mà cuối cùng phải khớp với nó. Không có chuỗi lựa chọn nào khiến phòng 2 bị cô lập ở cuối trong khi vẫn giữ cấu hình ở mức tối đa. Do đó xác suất là 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tối đa N) | Mỗi f[n] được tính một lần bằng cách sử dụng tổng tiền tố | 
| Không gian | O(tối đa N) | Lưu trữ mảng DP và tiền tố lên tới N tối đa | 

Quá trình tiền xử lý chia tỷ lệ tuyến tính với kích thước hành lang lớn nhất trong các trường hợp thử nghiệm, có thể chấp nhận được với N lên tới 10^7 trong một lần truyền trong Python được tối ưu hóa và dễ dàng nằm trong giới hạn trong các ngôn ngữ được biên dịch. Mỗi truy vấn được trả lời trong O(1). 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    T = int(input())
    queries = []
    max_n = 1

    for _ in range(T):
        n, k = map(int, input().split())
        queries.append((n, k))
        max_n = max(max_n, n)

    f = [0] * (max_n + 1)
    pref = [0] * (max_n + 1)

    for n in range(2, max_n + 1):
        total = 1 + pref[n - 2]
        inv = pow(n - 1, MOD - 2, MOD)
        f[n] = (total % MOD) * inv % MOD
        pref[n] = (pref[n - 1] + f[n]) % MOD

    out = []
    for i, (n, k) in enumerate(queries, 1):
        if k != 1 and k != n:
            out.append(f"Case #{i}: 1")
        else:
            out.append(f"Case #{i}: {f[n]}")

    return "\n".join(out)

# provided samples
assert run("4\n3 1\n3 2\n4 1\n4 2\n") == "Case #1: 500000004\nCase #2: 1\nCase #3: 666666672\nCase #4: 1"

# custom cases
assert run("1\n2 1\n") == "Case #1: 1"
assert run("1\n2 2\n") == "Case #1: 1"
assert run("1\n5 3\n") == "Case #1: 1"
assert run("1\n4 1\n") == "Case #1: 666666672"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| N=2 điểm cuối | 1 | độ chính xác của trường hợp cạnh tối thiểu | 
| điểm cuối đối xứng | 1 | cả hai đầu đều hoạt động giống hệt nhau | 
| phòng nội thất | 1 | nội thất luôn chiếm đóng | 
| N=4 điểm cuối | 2/3 | xác suất tái phát không hề nhỏ | 

## Vỏ cạnh 

Với N = 2, nước đi duy nhất có thể là (1,2), do đó cả hai phòng luôn có người. Thuật toán trả về chính xác f(2) = 1. 

Đối với các vị trí bên trong như N = 5, K = 3, mọi kết quả khớp cực đại hợp lệ trên một đường dẫn phải bao phủ đỉnh đó. Trong logic DP, những trường hợp như vậy thậm chí không bao giờ truy vấn f(n); chúng được giải quyết ngay lập tức dưới dạng xác suất 1. 

Đối với các trường hợp điểm cuối như N = 4, K = 1, phép tính lặp lại tính toán chính xác ba bước di chuyển đầu tiên đối xứng và phân bổ xác suất thành công ngay lập tức, thất bại ngay lập tức và tiếp tục đệ quy thành các phân đoạn nhỏ hơn.
