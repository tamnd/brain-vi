---
title: "CF 1739A - Hiệp Sĩ Bất Động"
description: "Chúng ta được cấp một bàn cờ rất nhỏ, nhiều nhất là 8 đô la nhân 8 đô la. Nhiệm vụ không phải là tính toán một giá trị phức tạp mà là xác định vị trí bất kỳ ô nào mà từ đó quân mã không có nước đi hợp pháp nào nằm trong bàn cờ."
date: "2026-06-09T17:41:42+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1739
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 136 (Rated for Div. 2)"
rating: 800
weight: 1739
solve_time_s: 265
verified: false
draft: false
---

[CF 1739A - Hiệp sĩ bất động](https://codeforces.com/problemset/problem/1739/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 4 phút 25s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một bàn cờ rất nhỏ, cùng lắm là$8 \times 8$. Nhiệm vụ không phải là tính toán một giá trị phức tạp mà là xác định vị trí bất kỳ ô nào mà từ đó quân mã không có nước đi hợp pháp nào nằm trong bàn cờ. Nếu một ô như vậy tồn tại, chúng ta phải xuất một trong số chúng; nếu không chúng ta được phép xuất bất kỳ tọa độ nào lên bảng. 

Đối tượng chính là chiêu thức hiệp sĩ tiêu chuẩn: từ một ô$(r, c)$, một hiệp sĩ cố gắng nhảy tới tám vị trí có thể có của hình dạng$(\pm 2, \pm 1)$Và$(\pm 1, \pm 2)$. Một ô được gọi là bị cô lập nếu không có điểm đến nào trong số tám điểm đến đó vẫn còn trong bảng. 

Vì cả hai chiều đều được giới hạn bởi$8$, tổng số ô nhiều nhất là$64$. Điều này ngay lập tức đặt vấn đề vào chế độ mà mô phỏng trực tiếp không chỉ đủ mà còn sạch hơn về mặt khái niệm so với bất kỳ tối ưu hóa nào. Bất kỳ phương pháp nào kiểm tra từng ô và kiểm tra tối đa tám nước đi đều chạy trong thời gian không đổi cho mỗi trường hợp thử nghiệm. 

Trường hợp cạnh tinh tế xuất hiện khi bo mạch cực kỳ nhỏ. Trên một$1 \times m$hoặc$n \times 1$bàn cờ, mọi ô đều bị cô lập vì mọi nước đi của hiệp sĩ đều rời khỏi bàn cờ. Trên một$2 \times 2$bảng, điều tương tự cũng xảy ra. Trên các bảng lớn hơn như$3 \times 3$, một số ô trung tâm bị cô lập trong khi các ô viền vẫn cho phép ít nhất một lần di chuyển hợp lệ, tùy thuộc vào hình học. Định nghĩa này hoàn toàn mang tính cục bộ, do đó không có vấn đề ràng buộc toàn cầu hoặc tính chẵn lẻ nào cần xem xét. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu sẽ kiểm tra từng tế bào và thử tất cả tám nước đi của hiệp sĩ. Nếu ít nhất một nước đi vẫn ở trong lưới thì ô đó không bị cô lập. Nếu không có, chúng tôi ngay lập tức trả lại nó. Vì có nhiều nhất$64$tế bào và$8$hướng dẫn trên mỗi ô, công việc trên mỗi trường hợp thử nghiệm bị giới hạn bởi vài trăm thao tác, tốc độ này rất nhanh. 

Không có cấu trúc sâu hơn để khai thác vì bảng quá nhỏ. Lý do duy nhất để suy nghĩ xa hơn là thừa nhận rằng điều kiện đó thuần túy mang tính hình học và cục bộ: sự cô lập chỉ phụ thuộc vào việc liệu môi trường xung quanh có$3 \times 3$Và$5 \times 5$các khu dân cư tồn tại trong giới hạn. Điều này có nghĩa là chúng ta không cần tiền xử lý hoặc tìm kiếm. 

Giải pháp tối ưu giống hệt với phương pháp vũ phu, chỉ được thực hiện trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nm)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 
| Tối ưu |$O(nm)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập và quét bảng. 

1. Lặp lại từng ô$(r, c)$trong$n \times m$lưới. Mỗi tế bào là một ứng cử viên để được cô lập. 
2. Đối với ô cố định, hãy kiểm tra tất cả tám nước đi của hiệp sĩ. Mỗi lần di chuyển tạo ra một ô mục tiêu$(r + dr, c + dc)$, và chúng tôi xác minh xem nó có nằm trong giới hạn bảng hay không$1 \le r \le n$,$1 \le c \le m$. 
3. Nếu ít nhất một bước di chuyển hợp lệ, hãy đánh dấu ô là không bị cô lập và tiếp tục đến bước tiếp theo. Điều này đảm bảo chúng tôi chỉ chấp nhận các ô không có chuyển đổi hiệp sĩ hợp lệ. 
4. Nếu chúng tôi tìm thấy một ô mà không có bước nào trong số tám bước di chuyển hợp lệ, chúng tôi sẽ xuất ngay ô đó và ngừng xử lý trường hợp kiểm thử hiện tại. 
5. Nếu không tìm thấy ô biệt lập sau khi quét tất cả các vị trí, hãy xuất bất kỳ ô nào, chẳng hạn$(1, 1)$. 

Ý tưởng chính là định nghĩa về sự cô lập hoàn toàn mang tính cục bộ và chỉ phụ thuộc vào vùng lân cận có thể tiếp cận được bằng các bước di chuyển của hiệp sĩ, vì vậy việc kiểm tra toàn diện không thể bỏ sót bất kỳ ứng cử viên hợp lệ nào. 

### Tại sao nó hoạt động 

Một ô được cô lập chính xác khi tất cả tám hiệp sĩ rời khỏi lưới. Thuật toán kiểm tra điều kiện này một cách trực tiếp. Vì mỗi ô được kiểm tra độc lập và tất cả các bước di chuyển có thể được liệt kê nên một ô được báo cáo là tách biệt khi và chỉ khi nó thỏa mãn định nghĩa. Nếu không có ô nào như vậy tồn tại, thì vấn đề cho phép trả về bất kỳ tọa độ nào, do đó tính chính xác được bảo toàn trong mọi trường hợp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    moves = [(2, 1), (2, -1), (-2, 1), (-2, -1),
             (1, 2), (1, -2), (-1, 2), (-1, -2)]
    
    for _ in range(t):
        n, m = map(int, input().split())
        
        found = False
        for r in range(1, n + 1):
            for c in range(1, m + 1):
                isolated = True
                
                for dr, dc in moves:
                    nr, nc = r + dr, c + dc
                    if 1 <= nr <= n and 1 <= nc <= m:
                        isolated = False
                        break
                
                if isolated:
                    print(r, c)
                    found = True
                    break
            
            if found:
                break
        
        if not found:
            print(1, 1)

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo thuật toán theo đúng nghĩa đen. Sự tinh tế duy nhất là chấm dứt sớm: khi tìm thấy một ô riêng biệt, chúng tôi sẽ ngừng quét thêm vì mọi câu trả lời hợp lệ đều được chấp nhận. Danh sách di chuyển là cố định và đầy đủ, đảm bảo tính chính xác mà không cần phân tích trường hợp. 

## Ví dụ đã hoạt động 

Hãy xem xét một$3 \times 3$Cái bảng. Chúng tôi kiểm tra ô trung tâm$(2, 2)$. Mọi bước di chuyển của hiệp sĩ từ nó đều đi ra ngoài lưới, vì vậy nó ngay lập tức được phân loại là bị cô lập. Thuật toán dừng ngay khi gặp ô này trong quá trình lặp. 

| Tế bào | Nước đi hiệp sĩ hợp lệ tồn tại | Bị cô lập | 
| --- | --- | --- | 
| (1,1) | Có | Không | 
| (1,2) | Có | Không | 
| (1,3) | Có | Không | 
| (2,2) | Không | Có | 

Điều này xác nhận rằng thuật toán xác định chính xác vị trí cách ly duy nhất trong trường hợp này. 

Bây giờ hãy xem xét một$2 \times 3$Cái bảng. Mỗi ô có ít nhất một nước đi hiệp sĩ nằm trong lưới, vì vậy không có ô biệt lập nào tồn tại. Thuật toán quét tất cả các vị trí, không tìm thấy vị trí nào bị cô lập và xuất ra$(1,1)$. 

| Tế bào | Nước đi hiệp sĩ hợp lệ tồn tại | Bị cô lập | 
| --- | --- | --- | 
| (1,1) | Có | Không | 
| (1,2) | Có | Không | 
| (1,3) | Có | Không | 
| (2,1) | Có | Không | 
| (2,2) | Có | Không | 
| (2,3) | Có | Không | 

Điều này thể hiện hành vi dự phòng khi tập hợp bị cô lập trống. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm)$mỗi bài kiểm tra | Mỗi ô kiểm tra tối đa tám nước đi liên tục | 
| Không gian |$O(1)$| Chỉ danh sách di chuyển cố định mới được lưu trữ | 

Kích thước bảng tối đa là$64$các ô, do đó, ngay cả tổng công việc trong trường hợp xấu nhất trên tất cả các trường hợp thử nghiệm cũng không đáng kể trong giới hạn. Việc sử dụng bộ nhớ là không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    def solve():
        t = int(input())
        moves = [(2, 1), (2, -1), (-2, 1), (-2, -1),
                 (1, 2), (1, -2), (-1, 2), (-1, -2)]
        
        for _ in range(t):
            n, m = map(int, input().split())
            
            found = False
            for r in range(1, n + 1):
                for c in range(1, m + 1):
                    isolated = True
                    for dr, dc in moves:
                        nr, nc = r + dr, c + dc
                        if 1 <= nr <= n and 1 <= nc <= m:
                            isolated = False
                            break
                    if isolated:
                        print(r, c)
                        found = True
                        break
                if found:
                    break
            if not found:
                print(1, 1)
    
    solve()
    return sys.stdout.getvalue().strip()

assert run("3\n1 7\n8 8\n3 3\n") == "1 1\n1 1\n2 2", "samples"

assert run("1\n1 1\n") == "1 1", "minimum board"
assert run("1\n2 2\n") == "1 1", "small full isolation"
assert run("1\n2 3\n") == "1 1", "no isolated cell case"
assert run("1\n3 3\n") == "2 2", "center isolated cell"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|$1 \times 1$bảng |$(1,1)$| sự cô lập tầm thường | 
|$2 \times 2$bảng |$(1,1)$| tất cả các tế bào bị cô lập | 
|$2 \times 3$bảng |$(1,1)$| không có tế bào biệt lập | 
|$3 \times 3$bảng |$(2,2)$| trường hợp cách ly trung tâm | 

## Vỏ cạnh 

Trên một$1 \times m$hoặc$n \times 1$bàn cờ, mọi ô đều bị cô lập vì tất cả các bước di chuyển của hiệp sĩ ngay lập tức vượt quá giới hạn trong ít nhất một tọa độ. Thuật toán xử lý việc này vì đối với mỗi ô, tất cả tám bước di chuyển ứng cử viên đều không vượt qua được việc kiểm tra ranh giới, do đó ô đầu tiên gặp phải sẽ được trả về dưới dạng biệt lập. 

Trên một$2 \times 2$bàn cờ, logic tương tự cũng được áp dụng, nhưng bây giờ ở dạng dày đặc hơn: mọi nước đi của hiệp sĩ đều rời khỏi lưới do không đủ chiều cao hoặc chiều rộng. Quá trình quét sẽ phân loại ô đầu tiên là bị cô lập và dừng ngay lập tức. 

Trên các bảng lớn hơn như$3 \times 3$, chỉ ô trung tâm không có nước đi hiệp sĩ hợp lệ bên trong lưới. Thuật toán tiếp cận nó trong quá trình lặp và xác định chính xác nó là bị cô lập vì mọi phần bù đều vượt quá giới hạn.
