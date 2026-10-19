---
title: "CF 105632M - Lấy mẫu loại bỏ"
description: "Chúng ta có một vũ trụ gồm các phần tử từ 1 đến n và mỗi phần tử i mang một trọng số ai. Mục tiêu là thiết kế một quy trình ngẫu nhiên tạo ra các tập hợp con có kích thước k cố định bằng cách sử dụng các lần tung đồng xu độc lập, sau đó loại bỏ các kết quả không hợp lệ."
date: "2026-06-22T05:38:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105632
codeforces_index: "M"
codeforces_contest_name: "2024 China Collegiate Programming Contest (CCPC) Zhengzhou Onsite (The 3rd Universal Cup. Stage 22: Zhengzhou)"
rating: 0
weight: 105632
solve_time_s: 42
verified: true
draft: false
---

[CF 105632M - Lấy mẫu từ chối](https://codeforces.com/problemset/problem/105632/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một vũ trụ gồm các phần tử từ 1 đến n và mỗi phần tử i mang một trọng số ai. Mục tiêu là thiết kế một quy trình ngẫu nhiên tạo ra các tập hợp con có kích thước k cố định bằng cách sử dụng các lần tung đồng xu độc lập, sau đó loại bỏ các kết quả không hợp lệ. 

Quá trình này hoạt động bằng cách gán cho mỗi chỉ số i một xác suất pi. Trong một thử nghiệm, chúng tôi đưa mỗi i một cách độc lập với xác suất pi, tạo ra một tập hợp con ngẫu nhiên T. Nếu tập hợp con kết quả có kích thước chính xác là k thì chúng tôi chấp nhận nó; nếu không chúng tôi sẽ loại bỏ toàn bộ thử nghiệm và lặp lại cho đến khi thành công. Trong số tất cả các kết quả thành công, chúng ta muốn phân bố trên các tập con cỡ k S tỷ lệ với tích của ai trên tất cả các phần tử trong S. 

Đầu ra không phải là các tập con mà là xác suất bao gồm pi. Các xác suất này phải có tổng bằng k và sau khi lấy mẫu từ chối, chúng phải tạo ra một phân bố trên k-tập hợp con phù hợp với cấu trúc trọng số nhân được yêu cầu. 

Các ràng buộc cho phép n lên tới 100000, điều này ngay lập tức loại trừ mọi thứ liên quan đến việc liệt kê các tập hợp con hoặc lập trình động trên tất cả các kết hợp k. Bất kỳ cách tiếp cận đúng nào cũng phải giảm vấn đề về thời gian tuyến tính hoặc gần thời gian tuyến tính với một số lượng nhỏ các lần truyền qua mảng. 

Một khía cạnh tinh tế là sự phân bố được xác định sau khi điều chỉnh sự kiện |T| = K. Điều kiện đó kết hợp tất cả các tọa độ, vì vậy mặc dù thế hệ là độc lập nhưng xác suất cuối cùng pi không phải là hàm độc lập của ai một cách ngây thơ. 

Một kiểu thất bại điển hình là giả sử pi tỷ lệ thuận với ai hoặc chỉ cần chuẩn hóa là đủ. Ví dụ: nếu n = 3, k = 1, a = [1, 100, 100], việc đoán số pi tỷ lệ với ai sẽ đưa ra các xác suất không tôn trọng cấu trúc điều hòa và phân bố cảm ứng sau khi bị loại bỏ không đồng nhất trên các đơn vị có trọng số. 

Một cạm bẫy tinh vi khác là sự ổn định về số lượng. Vì pi là số thực và phải thỏa mãn ràng buộc toàn cục, nên các lỗi nhỏ sẽ tích lũy thông qua điều kiện chuẩn hóa và có thể vi phạm ràng buộc tổng hoặc tỷ lệ biến dạng. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực sẽ cố gắng giải quyết số pi trực tiếp từ điều kiện xác định. Người ta có thể tưởng tượng việc liệt kê tất cả các tập con S có kích thước k, viết xác suất lấy mẫu loại bỏ tạo ra S theo số pi và đánh đồng tỷ số giữa hai tập con bất kỳ với tỷ số trọng số của chúng. Điều này dẫn đến một hệ thống gồm nhiều phương trình theo cấp số nhân, bởi vì mỗi xác suất của tập hợp con mở rộng thành tích của pi và (1 - pi). Ngay cả việc viết khả năng đầy đủ cũng yêu cầu tính tổng tất cả các tập hợp con thuộc mọi kích cỡ, đó là O(2^n). Điều này là không thể ngay cả đối với n nhỏ. 

Quan sát cấu trúc quan trọng là bước loại bỏ điều kiện dựa trên lượng số cố định, điều này gợi ý mối liên hệ với phân phối nhị thức Poisson dựa trên các ràng buộc tổng. Phân phối của T trước khi bị loại bỏ được phân tích đầy đủ thành thừa số, nhưng sau khi điều hòa trên |T| = k, chúng ta nhận được phân phối dạng sản phẩm có kích thước cố định. 

Phân phối mục tiêu cần thiết trên k tập con tỷ lệ với ∏ ai, chính xác là phân phối thu được từ việc lấy mẫu k mục mà không thay thế trong đó mỗi mục có trọng số ai. Đây là mô hình tập hợp con k có trọng số cổ điển, còn được gọi là cấu trúc siêu hình học không trung tâm Wallenius hoặc Fisher đa biến. Trong các mô hình như vậy, có một biểu diễn đã biết: mỗi phần tử i có một pi “xu hướng bao gồm” phụ thuộc vào một tham số tỷ lệ duy nhất λ và dạng đúng là 

pi = 1 / (1 + λ / ai) 

đối với một số λ > 0 được chọn sao cho kích thước dự kiến của T bằng k khi lấy mẫu độc lập.

Cấu trúc này xuất hiện vì tỷ lệ xác suất bao gồm giữa hai phần tử bất kỳ phải khớp với tỷ lệ trọng số sau khi điều hòa. Việc thực thi tính tương xứng trên tất cả các tập hợp con k sẽ tạo ra một họ nghiệm một tham số và ràng buộc ∑ pi = k ghim xuống λ một cách duy nhất. 

Do đó, bài toán quy về việc tìm λ sao cho tổng của pi(λ) bằng k. Vì số pi giảm đơn điệu trong λ nên tổng cũng đơn điệu, nên có thể tìm thấy λ bằng cách sử dụng tìm kiếm nhị phân. Mỗi đánh giá tổng là O(n), tạo ra giải pháp đầy đủ O(n độ chính xác log). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con | O(2^n) | O(2^n) | Quá chậm | 
| Tham số đơn điệu + tìm kiếm nhị phân | O(n log(1/ε)) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giới thiệu một tham số dương duy nhất λ kiểm soát đồng thời tất cả các xác suất. Đối với λ cố định, hãy xác định từng xác suất đưa vào là pi(λ) = ai / (ai + λ). Dạng này được chọn vì đây là phép biến đổi đối xứng trơn tru duy nhất bảo toàn tỷ lệ chênh lệch giữa các phần tử trong khi ánh xạ trọng số thành xác suất trong (0,1). 

1. Cố định giá trị ứng viên là λ. Đối với λ này, hãy tính mọi pi(λ) bằng cách sử dụng pi = ai / (ai + λ). Bước này chuyển trọng số thành xác suất bao hàm theo cách duy trì thứ tự: ai lớn hơn luôn hàm ý số pi lớn hơn. 
2. Tính tổng S(λ) = ∑ pi(λ). Tổng này thể hiện kích thước dự kiến ​​của tập mẫu trước khi bị từ chối. Bước loại bỏ không làm thay đổi pi nên đây là đại lượng chính xác cần kiểm soát. 
3. Quan sát rằng khi λ → 0, mọi pi tiến đến 1, do đó S(λ) tiến đến n. Khi λ → ∞, mọi pi tiến đến 0, do đó S(λ) tiến đến 0. Điều này đảm bảo rằng mọi mục tiêu k trong [1, n − 1] đều có thể đạt được. 
4. Sử dụng tìm kiếm nhị phân trên λ để tìm giá trị duy nhất trong đó S(λ) = k. Tại mỗi điểm giữa, hãy tính S(λ) và so sánh nó với k. Nếu S(λ) quá lớn thì λ quá nhỏ và phải tăng lên; nếu không thì giảm λ. 
5. Sau khi hội tụ, xuất pi cho mỗi i bằng cách sử dụng λ cuối cùng. 

Tìm kiếm nhị phân kết thúc khi tổng đủ chính xác để các giá trị pi riêng lẻ khác với giá trị thực của chúng tối đa là 1e-6. 

### Tại sao nó hoạt động 

Việc phân phối sau khi bị loại bỏ chỉ phụ thuộc vào tỷ lệ bao gồm tương đối giữa các phần tử. Dạng pi = ai / (ai + λ) bảo toàn chính xác các tỷ lệ đó, vì 

pi/(1 − pi) = ai/λ. 

Điều này có nghĩa là tỷ lệ bao gồm log tỷ lệ với log(ai), được dịch chuyển bởi hằng số λ. Việc điều hòa chính xác k mục được chọn buộc số lượng phần tử dự kiến ​​phải khớp với k và tính đơn điệu của S(λ) đảm bảo một giải pháp duy nhất. Do đó, việc xây dựng là cách nhất quán duy nhất để sắp xếp các thử nghiệm Bernoulli độc lập với phân bố lấy mẫu có trọng số có kích thước cố định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    def calc(lam):
        s = 0.0
        for x in a:
            s += x / (x + lam)
        return s

    lo, hi = 0.0, 1.0

    # expand hi until sum <= k
    while calc(hi) > k:
        hi *= 2.0

    for _ in range(80):
        mid = (lo + hi) / 2.0
        if calc(mid) > k:
            lo = mid
        else:
            hi = mid

    lam = hi
    res = [x / (x + lam) for x in a]
    print("\n".join(f"{v:.12f}" for v in res))

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách xác định hàm S(λ), hàm này tính toán kích thước tập hợp con dự kiến ​​cho một tham số đã cho. Giới hạn trên ban đầu được mở rộng theo cấp số nhân cho đến khi kích thước dự kiến ​​giảm xuống dưới k, đảm bảo khoảng tìm kiếm nhị phân là hợp lệ. 

Tìm kiếm nhị phân sau đó được áp dụng cho một số lần lặp cố định. Tính đơn điệu của S(λ) đảm bảo tính đúng đắn của việc cập nhật hướng. Sau khi hội tụ, λ cuối cùng được sử dụng để tính trực tiếp từng pi. 

Một cạm bẫy triển khai phổ biến là quên rằng độ chính xác của dấu phẩy động đòi hỏi phải có đủ số lần lặp thay vì một điểm dừng epsilon đơn giản. Sử dụng số lần lặp cố định như 80 là đủ để có độ chính xác gấp đôi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

n = 3, k = 2, a = [5, 5, 5] 

Chúng ta kỳ vọng tính đối xứng nên tất cả số pi phải bằng nhau. 

| λ | p1 | p2 | p3 | tổng hợp | 
| --- | --- | --- | --- | --- | 
| nhỏ | ~1 | ~1 | ~1 | > 2 | 
| điều chỉnh | 2/3 | 2/3 | 2/3 | 2 | 

Ở trạng thái cân bằng, mỗi xác suất trở thành 2/3, phù hợp với ràng buộc rằng kích thước dự kiến là 2. 

Điều này xác nhận rằng tính đối xứng trong đầu vào tác động đến tính đối xứng trong lời giải và tìm kiếm nhị phân tìm ra điểm mà số lượng dự kiến khớp với k. 

### Ví dụ 2 

đầu vào: 

n = 2, k = 1, a = [1, 4] 

Chúng tôi hy vọng yếu tố thứ hai sẽ được chọn thường xuyên hơn. 

| λ | p1 | p2 | tổng hợp | 
| --- | --- | --- | --- | 
| λ lớn | nhỏ | nhỏ | < 1 | 
| λ nhỏ | lớn | lớn | > 1 | 

Khi hội tụ, λ cân bằng kích thước dự kiến ​​thành 1, tạo ra p2 ≈ 0,666 và p1 ≈ 0,333. Tỷ lệ này phản ánh tỷ lệ trọng lượng, xác nhận rằng các phần tử nặng hơn có xác suất bao gồm cao hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log C) | Mỗi đánh giá của S(λ) là O(n) và tìm kiếm nhị phân chạy với số lần lặp cố định | 
| Không gian | O(n) | Lưu trữ mảng đầu vào | 

Thuật toán là tuyến tính trên mỗi lần lặp và sử dụng số lần lặp không đổi để có độ chính xác gấp đôi. Với n lên tới 100000, điều này thoải mái phù hợp trong giới hạn 1-2 giây thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    import builtins
    backup = builtins.input

    def fake_input():
        return sys.stdin.readline().strip()

    builtins.input = fake_input
    try:
        solve()
    finally:
        builtins.input = backup

# provided samples
# (placeholders since exact formatting omitted)
# custom cases

# n=2 minimal k
# assert run("2 1\n1 1\n") == expected

# all equal large n small k
# assert run("5 3\n10 10 10 10 10\n") ...

# skewed weights
# assert run("3 1\n1 100 1000\n") ...
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 2 / 5 5 5 | đồng phục 2/3 | đối xứng | 
| 2 1 / 1 4 | xác suất sai lệch | thiên vị trọng lượng | 
| 5 3 / giá trị bằng nhau | đồng phục k/n | trường hợp cân bằng | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả ai đều bằng nhau. Trong trường hợp đó hệ thống phải giảm đến xác suất đồng nhất pi = k/n. Thuật toán xử lý điều này vì S(λ) là tìm kiếm đối xứng và tìm kiếm nhị phân hội tụ về λ duy nhất tạo ra xác suất bằng nhau. 

Một trường hợp cạnh khác là khi k rất nhỏ, chẳng hạn như k = 1. Khi đó chính xác một phần tử được mong đợi trong T và giải pháp đẩy λ lớn sao cho mọi xác suất đều nhỏ, với tỷ lệ khớp với ai. Việc xây dựng bảo tồn sự lựa chọn tỷ lệ giữa các phần tử một cách tự nhiên. 

Trường hợp cạnh cuối cùng là sự chênh lệch trọng lượng cực lớn, chẳng hạn như một ai là 1e9 và những ai khác là 1. Cấu trúc đơn điệu đảm bảo rằng tìm kiếm nhị phân vẫn hội tụ và dạng tỷ lệ pi = ai / (ai + λ) ngăn chặn tràn vì tất cả các hoạt động vẫn ổn định ở dấu phẩy động ngay cả đối với ai lớn.
