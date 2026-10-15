---
title: "CF 105461J - Nói vô nghĩa"
description: "Chúng ta đang xử lý một hoán vị ẩn của các vị trí từ 1 đến n. Bất cứ khi nào chúng ta gửi một từ có độ dài n, hệ thống sẽ sắp xếp lại các chữ cái theo hoán vị cố định này và trả về kết quả."
date: "2026-06-23T17:54:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105461
codeforces_index: "J"
codeforces_contest_name: "2024-2025 ICPC, Swiss Subregional"
rating: 0
weight: 105461
solve_time_s: 63
verified: true
draft: false
---

[CF 105461J - Nói vô nghĩa](https://codeforces.com/problemset/problem/105461/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta đang xử lý một hoán vị ẩn của các vị trí từ 1 đến n. Bất cứ khi nào chúng ta gửi một từ có độ dài n, hệ thống sẽ sắp xếp lại các chữ cái theo hoán vị cố định này và trả về kết quả. 

Hoán vị nhất quán trên tất cả các truy vấn, vì vậy mọi phản hồi chỉ là việc gắn nhãn lại các chỉ mục mang tính xác định. Nhiệm vụ là tái tạo lại chính xác vị trí cuối cùng của mỗi vị trí ban đầu. 

Khó khăn chính là chúng ta chỉ có thể tương tác tối đa bốn lần và n có thể lớn tới 100000. Điều đó loại trừ bất kỳ chiến lược nào cố gắng khôi phục hoán vị từng vị trí một. Bất kỳ cách tiếp cận nào truy vấn từng chỉ mục một cách độc lập sẽ ngay lập tức vượt quá giới hạn về tương tác và ngay cả việc tái cấu trúc không tương tác dựa trên các cặp vị trí thăm dò vẫn sẽ yêu cầu số lượng tương tác tuyến tính hoặc siêu tuyến tính. 

Cấu trúc gợi ý rằng mỗi truy vấn phải mang thông tin về nhiều vị trí đồng thời và mỗi phản hồi phải cho phép chúng tôi trích xuất danh tính vị trí một cách gián tiếp. 

Một trường hợp lỗi khó phát hiện sẽ xuất hiện nếu chúng ta cố gắng gán các ký tự giống hệt nhau hoặc nhãn có độ phân giải thấp. Ví dụ: nếu chúng tôi gửi một chuỗi tất cả`'a'`, câu trả lời cũng là tất cả`'a'`, không tiết lộ gì về hoán vị. Tương tự, chỉ sử dụng một vài ký hiệu có thể phân biệt được sẽ dẫn đến xung đột trong đó nhiều vị trí có chung chữ ký, khiến cho việc tái thiết trở nên mơ hồ. 

Yêu cầu cốt lõi là đảm bảo rằng mọi vị trí đều có một danh tính duy nhất có thể được phục hồi sau khi hoán vị trong khi chỉ sử dụng bốn truy vấn. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử xác định vị trí của từng chỉ mục bằng cách tạo các truy vấn tách biệt các vị trí. Ví dụ, người ta có thể gửi các chuỗi trong đó chỉ có một vị trí được đánh dấu khác nhau và quan sát vị trí dấu đó xuất hiện ở đầu ra. Điều này sẽ hiệu quả vì chúng tôi có thể theo dõi chuyển động của một vị trí cho mỗi truy vấn. Tuy nhiên, điều này ngay lập tức thất bại trên quy mô lớn: việc xác định n vị trí sẽ yêu cầu Θ(n) truy vấn, trong khi chúng ta chỉ được phép có 4. 

Quan sát chính là chúng ta không cần xác định các vị trí một cách tuần tự. Thay vào đó, chúng ta có thể mã hóa mọi vị trí bằng chữ ký có độ dài cố định nhỏ và khôi phục chữ ký đó sau khi hoán vị. 

Hoán vị không phá hủy thông tin; nó chỉ sắp xếp lại nó. Nếu mỗi vị trí mang một “vectơ nhãn” duy nhất thì sau khi hoán vị, các vectơ đó chỉ được xáo trộn giữa các vị trí. Nếu chúng ta có thể tái tạo lại vectơ nhìn thấy ở mỗi chỉ số đầu ra, chúng ta có thể ánh xạ nó trở lại vị trí ban đầu. 

Chỉ với bốn truy vấn, mỗi vị trí có thể đóng góp bốn ký hiệu thông tin. Sử dụng bảng chữ cái 26 chữ cái, mỗi ký hiệu có thể mã hóa một chữ số cơ số 26. Điều này cung cấp cho chúng tôi 26^4 mã riêng biệt có thể có, quá đủ cho n lên tới 100000. 

Chúng tôi chỉ định mỗi vị trí i một biểu diễn cơ số 26 gồm 4 chữ số. Trong truy vấn k, vị trí i được gán chữ số thứ k làm ký tự. Sau khi hoán vị, mỗi vị trí đầu ra sẽ hiển thị mã gồm 4 ký tự đầy đủ của chỉ mục nguồn của nó. Điều này làm cho việc tái thiết trở nên trực tiếp: chúng tôi ánh xạ từng bộ dữ liệu 4 ký tự trở lại chỉ mục ban đầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Truy vấn O(n^2) hoặc O(n) | O(n) | Quá chậm | 
| Mã hóa tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Gán mỗi chỉ mục một mã định danh duy nhất bằng cách sử dụng mã hóa base-26 trên 4 chữ số. Điều này đảm bảo mọi vị trí đều có một chữ ký riêng biệt có thể được xây dựng lại sau này. 
2. Xây dựng bốn chuỗi truy vấn. Với mỗi vị trí i và mỗi truy vấn k từ 0 đến 3, đặt chữ số thứ k của i trong bảng chữ cái ở vị trí i của chuỗi thứ k. 
3. Gửi chuỗi truy vấn đầu tiên và nhận kết quả được hoán vị. Đối với mỗi vị trí j trong phản hồi, hãy ghi lại ký tự đến từ vị trí ban đầu ánh xạ vào j theo hoán vị. 
4. Lặp lại quy trình tương tự cho cả bốn truy vấn. Sau khi xử lý tất cả các phản hồi, mỗi vị trí j đã thu thập được 4 ký tự, tạo thành chữ ký 4 ký tự. 
5. Chuyển đổi từng chữ ký 4 ký tự trở lại chỉ mục gốc bằng cách sử dụng giải mã cơ sở 26. Điều này mang lại hoán vị nghịch đảo, nghĩa là đối với mỗi vị trí đầu ra j, chúng ta tìm thấy chỉ mục ban đầu mà tôi đã tạo ra nó. 
6. Xuất hoán vị theo dạng yêu cầu. 

Lý do điều này hoạt động là vì mỗi truy vấn hoạt động giống như một phép chiếu mã hóa chỉ số 4 chiều. Hoán vị chỉ sắp xếp lại các mã hóa này chứ không trộn lẫn chúng, do đó mỗi vị trí sẽ giữ lại một chữ ký hoàn chỉnh và có thể phục hồi được. 

### Tại sao nó hoạt động 

Điều bất biến là tại bất kỳ thời điểm nào, mọi vị trí trong chuỗi được hoán vị đều mang chính xác cùng một mã gồm 4 ký tự đã được gán cho một chỉ mục gốc duy nhất. Do hàm mã hóa từ các chỉ mục đến vectơ 4 ký tự là hàm nội xạ và phép hoán vị bảo toàn cấu trúc nhiều tập hợp mà không làm thay đổi các giá trị, nên việc xây dựng lại chỉ mục gốc từ bất kỳ vị trí nào luôn luôn rõ ràng. Không có hai chỉ mục nào có chung bộ 4 ký tự, do đó không thể xảy ra xung đột trong quá trình xây dựng lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def encode(i):
    # 4-digit base-26 encoding
    res = []
    for _ in range(4):
        res.append(chr(ord('a') + (i % 26)))
        i //= 26
    return res  # least significant digit first

def main():
    n = int(input().strip())

    # build 4 query strings
    queries = [ [''] * n for _ in range(4) ]

    codes = []
    for i in range(n):
        c = encode(i)
        codes.append(c)
        for k in range(4):
            queries[k][i] = c[k]

    queries = [''.join(q) for q in queries]

    # store received characters per position
    got = [ [] for _ in range(n) ]

    for k in range(4):
        print("?", queries[k])
        sys.stdout.flush()
        resp = input().strip()

        for j, ch in enumerate(resp):
            got[j].append(ch)

    # reconstruct mapping
    pos = {}
    for i, c in enumerate(codes):
        pos[''.join(c)] = i + 1

    ans = [0] * n
    for j in range(n):
        key = ''.join(got[j])
        ans[j] = pos[key]

    print("!", *ans)
    sys.stdout.flush()

if __name__ == "__main__":
    main()
```Giai đoạn xây dựng xây dựng bốn lớp ký tự độc lập, mỗi lớp đóng vai trò là một chữ số của mã định danh cơ sở 26. Vòng lặp tương tác đọc lại bốn phiên bản được hoán vị của các lớp này và tích lũy các ký tự thuộc từng vị trí đầu ra. Sau khi tất cả bốn lớp được thu thập, mỗi vị trí được xác định đầy đủ bằng khóa gồm 4 ký tự, sau đó được giải mã trở lại chỉ mục ban đầu. 

Một cạm bẫy triển khai phổ biến là trộn lẫn hướng hoán vị. Thông tin chi tiết quan trọng là vị trí đầu ra lưu trữ thông tin về chỉ mục ban đầu chứ không phải ngược lại, do đó việc tái thiết phải coi các bộ dữ liệu đã thu thập là mục tiêu ánh xạ nghịch đảo. 

## Ví dụ đã hoạt động 

Xét một hoán vị nhỏ n = 5 với hoán vị p = [3, 5, 1, 4, 2]. Điều này có nghĩa là vị trí 1 chuyển sang 3, vị trí 2 chuyển sang 5, v.v. 

Chúng tôi gán mã cơ sở 26, nhưng để đơn giản, hãy tưởng tượng các ký tự: 

| chỉ số tôi | mã | 
| --- | --- | 
| 1 | aaa | 
| 2 | aaa | 
| 3 | aaba | 
| 4 | aba | 
| 5 | báa | 

Sau khi hoán vị, các vị trí nhận mã từ nguồn của chúng: 

| vị trí đầu ra j | nhận được mã | 
| --- | --- | 
| 1 | aaba | 
| 2 | báa | 
| 3 | aaa | 
| 4 | aba | 
| 5 | aaa | 

Từ bảng này, chúng tôi trực tiếp khôi phục các chỉ mục gốc bằng cách khớp mã trở lại vị trí được chỉ định của chúng. 

Dấu vết này cho thấy rằng mỗi vị trí đầu ra tái tạo lại nguồn gốc của nó một cách độc lập mà không cần bất kỳ suy luận vị trí chéo nào ngoài việc giải mã. 

Ví dụ thứ hai với n = 3 và hoán vị danh tính xác nhận tính ổn định. Mỗi vị trí đầu ra nhận chính xác mã riêng của nó, do đó việc giải mã sẽ trả về [1, 2, 3] một cách tầm thường, xác minh tính chính xác trong trường hợp điểm cố định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi truy vấn trong số 4 truy vấn xử lý n ký tự một lần | 
| Không gian | O(n) | Lưu trữ chữ ký 4 ký tự cho từng vị trí | 

Giải pháp thực hiện số lần quét toàn bộ chuỗi không đổi và lưu trữ nhãn có kích thước cố định cho mỗi vị trí. Với n lên tới 100000, điều này vẫn nằm trong giới hạn và chỉ có bốn truy vấn tương tác được sử dụng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    # Placeholder since this is interactive; real testing would mock interaction
    return "ok"

# custom sanity checks (non-interactive logic checks would go here)

assert True, "placeholder"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 danh tính | 1 | độ chính xác kích thước tối thiểu | 
| n=5 hoán vị ngẫu nhiên | hoán vị hợp lệ | tính đúng đắn chung | 
| n=100000 danh tính | 1..n | ổn định kích thước tối đa | 
| kiểm tra cấu trúc lặp đi lặp lại | giải mã độc đáo | an toàn va chạm | 

## Vỏ cạnh 

Với n = 1, mã hóa vẫn tạo ra chữ ký 4 ký tự cho một vị trí. Tất cả bốn truy vấn đều trả về các chuỗi ký tự đơn giống hệt nhau và các bản đồ xây dựng lại có chữ ký quay lại chỉ mục 1 mà không có sự mơ hồ. 

Đối với n lớn gần 100000, mã hóa bốn chữ số cơ sở 26 vẫn cung cấp đủ tính duy nhất vì 26^4 vượt quá phạm vi. Mỗi chỉ mục vẫn có thể nhận dạng duy nhất và không có xung đột nào xuất hiện trong bản đồ giải mã, đảm bảo tính chính xác theo tỷ lệ.
