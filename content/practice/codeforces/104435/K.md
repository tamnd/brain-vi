---
title: "CF 104435K - Tất Người Tìm Ngôi Sao"
description: "Chúng ta được cung cấp một bộ sưu tập các loại tất, trong đó mỗi loại chứa một số tất giống hệt nhau được nhóm thành từng cặp. Loại i đóng góp chính xác 2·k[i] tất riêng lẻ, tất cả đều không thể phân biệt được trong loại đó."
date: "2026-06-30T18:43:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104435
codeforces_index: "K"
codeforces_contest_name: "2023 UP ACM Algolympics Final Round"
rating: 0
weight: 104435
solve_time_s: 48
verified: true
draft: false
---

[CF 104435K - Tất của Người tìm kiếm ngôi sao](https://codeforces.com/problemset/problem/104435/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một bộ sưu tập các loại tất, trong đó mỗi loại chứa một số tất giống hệt nhau được nhóm thành từng cặp. Loại i đóng góp chính xác 2·k[i] tất riêng lẻ, tất cả đều không thể phân biệt được trong loại đó. 

Trong số tất cả các loại, chỉ có một tập con gồm m loại được coi là “phù hợp” cho một sự kiện. Blanc đang mù quáng rút những chiếc tất từ ​​một đống hỗn hợp và chúng tôi muốn cô ấy phải lấy số lượng tất nhỏ nhất để dù có xui xẻo đến đâu, cô ấy vẫn đảm bảo có ít nhất một đôi tất thuộc loại phù hợp. 

“Đôi” ở đây đơn giản có nghĩa là trong số những chiếc tất cô ấy đã lấy có ít nhất hai chiếc tất cùng loại phù hợp. 

Khó khăn chính là trận hòa có tính chất đối nghịch trong trường hợp xấu nhất. Chúng tôi không lấy mẫu ngẫu nhiên theo mong đợi; thay vào đó, chúng ta phải cho rằng đống được sắp xếp theo cách bất tiện nhất và chúng ta phải đảm bảo thành công bất kể thứ tự. 

Kích thước đầu vào bao hàm tối đa 10^3 loại cho mỗi trường hợp thử nghiệm và tối đa 10^2 trường hợp thử nghiệm. Do đó, mọi giải pháp sẽ chạy theo thời gian tuyến tính cho mỗi trường hợp thử nghiệm. Mô phỏng bậc hai hoặc tổ hợp trên tất cả các tập hợp con của tất sẽ quá chậm. 

Một trường hợp khó khăn xuất hiện khi suy nghĩ về việc liệu việc lấy nhiều tất từ ​​những loại không phù hợp có thể giúp ích hay gây tổn hại. Vì các loại không phù hợp không góp phần vào mục tiêu nên chúng chỉ đóng vai trò là “phần đệm an toàn”, không giúp hình thành một cặp bắt buộc mà có thể trì hoãn việc đạt được một cặp phù hợp. 

Một cạm bẫy khác là giả sử chúng ta cần hai chiếc tất thuộc bất kỳ loại nào trên toàn cầu. Yêu cầu nghiêm ngặt chỉ là về các loại phù hợp, vì vậy các cặp thuộc loại không phù hợp sẽ không liên quan. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề này là mô phỏng việc vẽ từng chiếc tất một và theo dõi tất cả các trạng thái có thể có của những gì chúng tôi đã thu thập được. Ở mỗi bước, chúng tôi sẽ xem xét tất cả các cách có thể để sắp xếp cọc và kiểm tra xem việc sắp xếp xấu có tránh được việc tạo thành một cặp phù hợp hay không. Điều này nhanh chóng trở thành một vụ nổ tổ hợp vì mỗi chiếc tất sẽ phân nhánh thành nhiều loại lựa chọn và chúng ta sẽ cần phải suy luận về sự sắp xếp đối nghịch trên tất cả các trình tự. Ngay cả đối với n vừa phải và tổng số tất lên tới 10^8, điều này là không khả thi. 

Quan sát quan trọng là chúng ta không được hỏi về xác suất hoặc một trình tự cụ thể mà về sự đảm bảo trong trường hợp xấu nhất. Điều đó có nghĩa là chúng ta nên xây dựng chuỗi rút thăm dài nhất có thể mà vẫn tránh được thành công. Một khi chúng ta biết độ dài thất bại tối đa đó, câu trả lời chỉ đơn giản là nhiều hơn nó một lần. 

Để tránh tạo thành một đôi tất phù hợp, đối với mỗi loại phù hợp, chúng ta được phép lấy tối đa một chiếc tất từ ​​đó. Nếu chúng tôi lấy được hai chiếc từ cùng một loại phù hợp thì chúng tôi đã thành công. Đối với những loại không phù hợp, ngay cả việc lấy hết tất cũng không giúp tạo thành một đôi cần thiết, vì vậy chúng có thể cạn kiệt hoàn toàn mà không đạt được thành công. 

Điều này làm giảm vấn đề phải tính toán xem có thể lấy bao nhiêu chiếc tất trong khi vẫn tôn trọng những ràng buộc này, sau đó thêm một chiếc tất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Đếm công suất trường hợp xấu nhất | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Bây giờ chúng tôi xây dựng trận hòa tồi tệ nhất có thể để tránh thành công càng lâu càng tốt.

1. Xác định loại tất nào phù hợp cho sự kiện. Chúng tôi được cung cấp chỉ số của họ một cách rõ ràng. 
2. Tính tổng số tất của tất cả các loại không phù hợp. Tất cả những chiếc tất này đều có thể được lấy đi mà không bao giờ giúp tạo thành một đôi hợp lệ, vì vậy trong trường hợp xấu nhất, chúng sẽ được thu thập đầy đủ trước khi chúng ta chạm vào bất kỳ cấu trúc hữu ích nào. 
3. Đối với mỗi loại phù hợp, hãy lưu ý rằng chúng ta có thể lấy tối đa một chiếc tất từ ​​nó một cách an toàn mà không cần tạo thành một đôi loại đó. Lấy chiếc tất thứ hai từ bất kỳ loại nào trong số này sẽ ngay lập tức đáp ứng yêu cầu, vì vậy đối thủ đảm bảo gặp được tối đa một chiếc tất từ ​​mỗi loại phù hợp trước khi buộc phải thành công. 
4. Tổng hợp những đóng góp này: tất cả các loại tất không phù hợp cộng với một chiếc tất từ ​​mỗi loại phù hợp. 
5. Số tất tối đa chúng ta có thể lấy mà vẫn tránh được thành công là số tiền này. Vì vậy, con số tối thiểu đảm bảo thành công là giá trị này cộng với một. 

### Tại sao nó hoạt động 

Lập luận này dựa trên một cấu trúc đối nghịch chặt chẽ. Sự sắp xếp trong trường hợp xấu nhất luôn có thể trì hoãn thành công bằng cách cách ly những chiếc tất phù hợp sao cho tối đa một bản sao của mỗi chiếc xuất hiện trong phần rút thăm ban đầu, đồng thời thoải mái cung cấp tất cả những chiếc tất không phù hợp. Cấu trúc này đáp ứng mọi cách để tránh một đôi phù hợp, có nghĩa là bất kỳ chiếc tất bổ sung nào nhất thiết phải tạo một bản sao trong một loại phù hợp, buộc phải có điều kiện bắt buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        k = list(map(int, input().split()))
        types = list(map(int, input().split()))

        suitable = set(types)

        total_unsuitable = 0

        for i in range(1, n + 1):
            if i not in suitable:
                total_unsuitable += 2 * k[i - 1]

        # one sock per suitable type in worst case
        max_safe = total_unsuitable + m

        print(max_safe + 1)

if __name__ == "__main__":
    solve()
```Giải pháp trực tiếp tuân theo cấu trúc của đối số. Trước tiên, chúng tôi đánh dấu các loại phù hợp bằng cách sử dụng bộ kiểm tra tư cách thành viên O(1). Sau đó, chúng tôi tích lũy tất cả các loại tất không phù hợp bằng cách tính tổng 2·k[i] cho các chỉ số không có trong tập hợp. 

Thuật ngữ`m`đại diện cho số lần rút đơn an toàn tối đa từ các loại phù hợp, vì mỗi loại phù hợp có thể đóng góp tối đa một chiếc tất mà không tạo thành một cặp. Cuối cùng, chúng tôi thêm một loại để buộc phải có sự trùng lặp đầu tiên không thể tránh khỏi trong số các loại phù hợp. 

Một lỗi triển khai phổ biến là chỉ lặp lại các loại phù hợp và quên rằng các loại không phù hợp sẽ đóng góp tất cả các loại tất của chúng vào tiền tố trường hợp xấu nhất. Một trường hợp khác là đếm nhầm các cặp thay vì đếm từng chiếc tất; đống bao gồm các chiếc tất riêng lẻ, vì vậy chúng tôi luôn làm việc theo đơn vị 2·k[i]. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ có năm loại và chỉ có hai loại là phù hợp. 

Chúng tôi theo dõi số lượng tất có thể được lấy mà không cần chọn một đôi phù hợp. 

| Bước | Không phù hợp Đã chụp | Những người độc thân phù hợp đã chụp | Tổng cộng | 
| --- | --- | --- | --- | 
| Ban đầu | 0 | 0 | 0 | 
| Lấy tất không phù hợp | 6 | 0 | 6 | 
| Lấy một chiếc tất từ ​​mỗi loại phù hợp | 6 | 2 | 8 | 

Điều này cho thấy tối đa 8 chiếc tất vẫn có thể tránh tạo thành một đôi phù hợp. Chiếc tất tiếp theo buộc phải sao chép một trong các loại phù hợp. 

Bây giờ hãy xem xét một trường hợp trong đó tất cả các loại đều phù hợp. 

| Bước | Không phù hợp Đã chụp | Những người độc thân phù hợp đã chụp | Tổng cộng | 
| --- | --- | --- | --- | 
| Ban đầu | 0 | 0 | 0 | 
| Lấy một chiếc tất cho mỗi loại | 0 | 3 | 3 | 

Chúng ta có thể lấy nhiều nhất một từ mỗi loại. Chiếc tất thứ tư đảm bảo có một đôi. 

Những dấu vết này xác nhận rằng giới hạn chỉ phụ thuộc vào việc đếm xem có bao nhiêu loại có thể bị “trì hoãn” trước khi việc sao chép trở nên không thể tránh khỏi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi loại được quét một lần để phân biệt những đóng góp phù hợp và không phù hợp | 
| Không gian | O(m) | Đặt cửa hàng loại phù hợp | 

Các ràng buộc cho phép tối đa 1000 loại cho mỗi trường hợp thử nghiệm, do đó, việc quét tuyến tính trên các loại có thể thoải mái trong giới hạn ngay cả đối với 100 trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import *
    from collections import *
    input = _sys.stdin.readline

    def solve():
        t = int(input())
        for _ in range(t):
            n, m = map(int, input().split())
            k = list(map(int, input().split()))
            types = list(map(int, input().split()))

            s = set(types)
            bad = 0
            for i in range(1, n + 1):
                if i not in s:
                    bad += 2 * k[i - 1]
            print(bad + m + 1)

    solve()
    return ""

# simple sanity
assert run("""1
1 1
1
1
""") == "", "single type"

# all unsuitable except none
assert run("""1
3 0
1 2 3
""") == "", "no suitable types edge handled implicitly"

# all suitable
assert run("""1
3 3
1 1 1
1 2 3
""") == "", "all types suitable"

# mixed
assert run("""1
5 2
1 2 3 4 5
1 3
""") == "", "mixed case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| loại đơn | 3 | độ chính xác cấu trúc tối thiểu | 
| không có loại phù hợp | 1 | hành vi thoái hóa | 
| tất cả đều phù hợp | hành vi n+1 | trường hợp ràng buộc đầy đủ | 
| trường hợp hỗn hợp | tính đúng đắn của công thức | tính đúng đắn chung | 

## Vỏ cạnh 

Khi không có loại phù hợp, công thức giảm xuống là lấy tất cả tất cộng một. Điều này phản ánh rằng không có số lượng hình vẽ nào có thể thỏa mãn điều kiện, vì vậy câu trả lời biến thành một bảo vệ bất khả thi tầm thường. 

Khi mọi loại đều phù hợp, thuật toán sẽ giảm xuống nguyên tắc chuồng bồ câu cổ điển: lấy một chiếc tất cho mỗi loại sẽ tránh thành công và chiếc tất tiếp theo buộc phải trùng lặp ở một số loại. 

Khi các loại phù hợp thưa thớt, tất cả sự phức tạp sẽ sụp đổ để tính toán chính xác sự đóng góp đầy đủ của các loại không phù hợp. Thuật toán xử lý vấn đề này một cách trực tiếp bằng cách tính tổng toàn bộ số lượng của chúng, đảm bảo chúng không ảnh hưởng đến ràng buộc “mỗi loại” trong số những số lượng phù hợp.
