---
title: "CF 103486E - Đại Thám Tử TJC"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi vị trí có một mảng các số nguyên dương và nhiệm vụ là xác định xem liệu chúng ta có thể chọn hai vị trí khác nhau sao cho các giá trị tại các vị trí đó khác nhau đúng một bit trong hệ nhị phân hay không, cụ thể là XOR của chúng bằng 1."
date: "2026-07-03T06:20:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103486
codeforces_index: "E"
codeforces_contest_name: "The 15th Jilin Provincial Collegiate Programming Contest"
rating: 0
weight: 103486
solve_time_s: 37
verified: true
draft: false
---

[CF 103486E - Thám tử vĩ đại TJC](https://codeforces.com/problemset/problem/103486/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi vị trí có một mảng các số nguyên dương và nhiệm vụ là xác định xem liệu chúng ta có thể chọn hai vị trí khác nhau sao cho các giá trị tại các vị trí đó khác nhau đúng một bit trong hệ nhị phân hay không, cụ thể là XOR của chúng bằng 1. 

Nói$A_i \oplus A_j = 1$là rất hạn chế. XOR bằng 1 có nghĩa là chỉ có bit có ý nghĩa nhỏ nhất khác nhau giữa hai số và mọi bit khác đều giống hệt nhau. Nói cách khác, một số phải chính xác là số kia với bit cuối cùng được lật. Vì vậy, các cặp hợp lệ luôn trông giống như$x$Và$x \oplus 1$. 

Kích thước đầu vào có thể lớn trong các trường hợp thử nghiệm, lên tới$5 \times 10^5$tổng số phần tử. Điều đó ngay lập tức loại trừ mọi cách tiếp cận so sánh tất cả các cặp, vì ngay cả một thử nghiệm trong trường hợp xấu nhất với$10^5$các yếu tố sẽ dẫn đến khoảng$10^{10}$kiểm tra cặp. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các số đều giống hệt nhau. Ví dụ, nếu mảng là$[7, 7, 7]$, không có cặp nào có thể hoạt động được vì$7 \oplus 7 = 0$, không phải 1. Một trường hợp khác là khi các số khác nhau nhiều hơn một bit chẳng hạn$[2, 3]$, Ở đâu$2 \oplus 3 = 1$hoạt động, nhưng$[2, 4]$không, vì$2 \oplus 4 = 6$. 

Khó khăn cốt lõi là nhận ra rằng điều kiện không phải là về cấu trúc XOR tùy ý mà là về mối quan hệ lân cận rất cụ thể giữa các số nguyên. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là thử tất cả các cặp$(i, j)$và tính toán XOR của chúng. Điều này đúng vì nó kiểm tra toàn diện mọi cặp ứng cử viên có thể có và chắc chắn sẽ tìm thấy một cặp hợp lệ nếu nó tồn tại. Tuy nhiên, đối với$n = 10^5$, điều này đòi hỏi khoảng$5 \times 10^9$hoạt động trong một trường hợp thử nghiệm, vượt xa mọi giới hạn khả thi. 

Quan sát quan trọng là chúng ta thực sự không cần phải so sánh các cặp tùy ý. Nếu một số$x$tham gia vào một cặp hợp lệ, đối tác của nó phải chính xác$x \oplus 1$. Điều này biến vấn đề thành vấn đề kiểm tra tư cách thành viên thay vì vấn đề ghép nối. Thay vì tìm kiếm cả hai đầu bằng cách quét, chúng ta có thể lưu trữ tất cả các giá trị trong cấu trúc dựa trên hàm băm và kiểm tra xem mỗi giá trị có$x$có bản sao của nó$x \oplus 1$có mặt trong mảng. 

Điều này làm giảm vấn đề từ so sánh bậc hai đến tra cứu theo thời gian liên tục cho mỗi phần tử. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra cặp Brute Force |$O(n^2)$|$O(1)$| Quá chậm | 
| Tra cứu bộ băm |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp kiểm thử, hãy đọc mảng và chèn tất cả các phần tử vào tập băm. 

Điều này cho phép truy vấn thành viên theo thời gian liên tục đối với bất kỳ giá trị nào mà chúng tôi có thể cần kiểm tra. 
2. Lặp qua từng phần tử$x$trong mảng. 

Với mỗi giá trị hãy tính$x \oplus 1$, đây là giá trị duy nhất có thể tạo thành một cặp hợp lệ với$x$. 
3. Kiểm tra xem$x \oplus 1$tồn tại trong tập hợp. 

Nếu đúng như vậy, chúng tôi biết ngay rằng có một cặp hợp lệ tồn tại và có thể ngừng xử lý trường hợp kiểm thử hiện tại. 
4. Nếu chúng tôi quét xong mảng mà không tìm thấy bất kỳ cặp nào như vậy, hãy kết luận rằng không tồn tại chỉ mục hợp lệ nào. 

Lựa chọn thiết kế chính là tính toán trước thành viên trong một tập hợp thay vì cố gắng tìm kiếm động bên trong mảng mỗi lần, việc này sẽ quét lại cấu trúc nhiều lần. 

### Tại sao nó hoạt động 

điều kiện$A_i \oplus A_j = 1$xác định duy nhất một giá trị từ giá trị kia. Đối với bất kỳ cố định$x$, có chính xác một đối tác ứng cử viên$x \oplus 1$và không có giá trị nào khác có thể thỏa mãn phương trình với$x$. Điều này có nghĩa là mọi cặp hợp lệ phải xuất hiện dưới dạng hai phần tử bổ sung trực tiếp trong phép biến đổi$x \mapsto x \oplus 1$. Bằng cách kiểm tra tất cả các phần tử về sự hiện diện của phần bù của chúng, chúng ta sẽ sử dụng hết tất cả các cách xây dựng hợp lệ có thể có mà không bị trùng lặp hoặc bỏ sót. 

## Giải pháp Python```
PythonRun
```Giải pháp đọc từng trường hợp thử nghiệm một cách độc lập và xây dựng tập băm gồm các giá trị mảng. Hoạt động quan trọng là XOR với 1, trực tiếp xây dựng giá trị đối tác duy nhất có thể có cho mỗi phần tử. Vòng lặp kết thúc sớm ngay khi tìm thấy cặp hợp lệ, giúp duy trì thời gian chạy hiệu quả trong các trường hợp thuận lợi. 

Một điểm tinh tế là chúng ta không cần phải lo lắng về việc ghép một phần tử với chính nó. Từ$x \oplus x = 0$, việc tự ghép nối không bao giờ có thể thỏa mãn điều kiện và việc tra cứu thiết lập sẽ tránh được điều này một cách tự nhiên trừ khi$x \oplus 1 = x$, điều này không bao giờ xảy ra với số nguyên. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào:```

```| x | x ⊕ 1 | trong bộ? | quyết định | 
| --- | --- | --- | --- | 
| 2 | 3 | vâng | tìm thấy | 

Ở đây, 2 và 3 tạo thành một cặp hợp lệ ngay lập tức vì XOR của chúng bằng 1. Thuật toán dừng ngay khi phát hiện ra mối quan hệ này. 

Bây giờ hãy xem xét:```
1
3
5 6 7
```| x | x ⊕ 1 | trong bộ? | quyết định | 
| --- | --- | --- | --- | 
| 5 | 4 | không | tiếp tục | 
| 6 | 7 | vâng | tìm thấy | 

Trong trường hợp này, cặp (6, 7) thỏa mãn điều kiện nên thuật toán sẽ phát hiện ra nó khi xử lý 6. 

Những dấu vết này cho thấy rằng chúng ta không tìm kiếm các mối quan hệ tùy ý mà chỉ tìm kiếm các lân cận bắt buộc duy nhất cho mỗi phần tử. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | Mỗi phần tử được chèn vào một tập hợp và được kiểm tra một lần | 
| Không gian |$O(n)$| Tập hợp này lưu trữ tất cả các phần tử của mảng | 

Tổng số phần tử trong tất cả các trường hợp thử nghiệm được giới hạn bởi$5 \times 10^5$, do đó thuật toán chạy thoải mái trong giới hạn. Kiểm tra tư cách thành viên dựa trên hàm băm giúp mỗi hoạt động luôn hiệu quả và liên tục. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    T = int(input())
    out = []
    for _ in range(T):
        n = int(input())
        arr = list(map(int, input().split()))
        s = set(arr)
        ok = False
        for x in arr:
            if (x ^ 1) in s:
                ok = True
                break
        out.append("Yes" if ok else "No")
    return "\n".join(out)

# provided sample (illustrative)
assert run("1\n2\n2 3\n") == "Yes"

# all equal
assert run("1\n4\n7 7 7 7\n") == "No"

# single pair
assert run("1\n2\n4 5\n") == "Yes"

# no pair
assert run("1\n3\n1 2 4\n") == "No"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 7 7 7 7 | Không | trùng lặp không tạo ra kết quả dương tính giả | 
| 4 5 | Có | cặp hợp lệ tối thiểu | 
| 1 2 4 | Không | các giá trị không liên quan không chính xác | 
| 2 3 | Có | XOR trực tiếp=1 trường hợp | 

## Vỏ cạnh 

Đối với một mảng trong đó tất cả các phần tử đều giống hệt nhau, chẳng hạn như$[10, 10, 10]$, mỗi lần kiểm tra đều tính$10 \oplus 1 = 11$, không có trong tập hợp, do đó thuật toán kết luận chính xác rằng không có cặp hợp lệ. 

Đối với một đầu vào tối thiểu như$[0, 1]$, kiểm tra ngay lập tức phát hiện ra rằng$0 \oplus 1 = 1$và vì cả hai đều tồn tại nên thuật toán trả về Có mà không cần quét thêm. Điều này xác nhận rằng sớm t
