---
title: "CF 104931E - Kết hợp lên xuống"
description: "Chúng tôi được cung cấp nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm bao gồm một hàng người đứng thành một hàng, trong đó mỗi người thuộc một trong hai nhóm, được mã hóa là U hoặc D."
date: "2026-06-28T07:38:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104931
codeforces_index: "E"
codeforces_contest_name: "UTPC Contest 01-26-24 Div. 1 (Advanced)"
rating: 0
weight: 104931
solve_time_s: 251
verified: false
draft: false
---

[CF 104931E - So khớp từ trên xuống](https://codeforces.com/problemset/problem/104931/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 11s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm bao gồm một hàng người đứng thành một hàng, trong đó mỗi người thuộc một trong hai nhóm, được mã hóa là`U`hoặc`D`. Nhiệm vụ là chọn một đoạn liền kề của hàng này sao cho bên trong đoạn đó có số lượng`U`ký tự chính xác bằng số lượng ký tự`D`các ký tự và trong số tất cả các phân đoạn hợp lệ như vậy, chúng tôi muốn có độ dài tối đa có thể. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi chỉ xuất ra một số duy nhất, độ dài của đoạn cân bằng dài nhất. Nếu không có phân đoạn nào không trống có thể thỏa mãn điều kiện thì câu trả lời là 0. 

Các ràng buộc ngụ ý rằng chúng ta có thể thấy tới mười nghìn trường hợp thử nghiệm, nhưng tổng chiều dài trên tất cả các chuỗi bị giới hạn bởi hai trăm nghìn. Điều đó buộc mọi giải pháp phải chạy theo thời gian tuyến tính ở mức trung bình cho mỗi trường hợp thử nghiệm, vì mọi phương trình bậc hai trên mỗi chuỗi sẽ ngay lập tức vượt quá giới hạn thời gian khi tất cả các trường hợp thử nghiệm đều lớn. 

Một cách tiếp cận đơn giản để kiểm tra mọi mảng con sẽ kiểm tra khoảng n phân đoạn bình phương cho mỗi trường hợp thử nghiệm. Với tổng số n lên đến hai trăm nghìn, điều này vượt xa khả năng thực hiện. 

Một vấn đề tế nhị xuất hiện khi tất cả các nhân vật đều thuộc một nhóm. Ví dụ,`UUUUU`hoặc`DDDDD`. Trong những trường hợp này, không có đoạn có độ dài dương hợp lệ nên câu trả lời phải bằng 0. Bất kỳ phương thức nào giả định tồn tại ít nhất một cặp hợp lệ sẽ trả về sai giá trị khác 0. 

Một trường hợp góc khác là khi đoạn tốt nhất kéo dài toàn bộ chuỗi. Ví dụ`UDUD`được cân bằng hoàn toàn và câu trả lời đúng là độ dài đầy đủ. Một giải pháp chỉ theo dõi các trận đấu cục bộ hoặc đặt lại quá mạnh sẽ bỏ lỡ cấu trúc toàn cầu này. 

## Phương pháp tiếp cận 

Chiến lược brute-force thử mọi chỉ số bắt đầu có thể và mở rộng đến mọi chỉ số kết thúc, đếm xem có bao nhiêu`U`Và`D`các ký tự xuất hiện trong mỗi phân đoạn. Bất cứ khi nào số lượng khớp, chúng tôi sẽ cập nhật câu trả lời tốt nhất. Điều này đúng vì nó kiểm tra rõ ràng mọi phân đoạn có thể. 

Tuy nhiên, việc duy trì số lượng cho từng cặp vẫn tốn thời gian tuyến tính trên mỗi phân đoạn nếu được tính toán lại hoặc thời gian không đổi nếu được duy trì tăng dần. Ngay cả trong cách triển khai tốt nhất, chúng tôi vẫn kiểm tra các phân đoạn O(n^2) cho mỗi trường hợp thử nghiệm. Với tổng kích thước đầu vào lên tới 200.000, điều này dẫn đến khoảng 20 tỷ thao tác trong trường hợp xấu nhất, điều này không khả thi. 

Quan sát quan trọng là chúng ta không thực sự quan tâm đến số lượng chính xác của`U`Và`D`riêng. Chúng tôi chỉ quan tâm đến sự khác biệt của họ. Nếu chúng ta diễn giải`U`là +1 và`D`là -1, thì một đoạn được cân bằng chính xác khi tổng của nó bằng 0. 

Điều này chuyển vấn đề thành việc tìm mảng con dài nhất có tổng bằng 0. Sử dụng tổng tiền tố, điều này trở nên tương đương với việc tìm hai tổng tiền tố bằng nhau ở vị trí i và j. Nếu tiền tố[i] bằng tiền tố[j] thì mảng con (i+1 đến j) được cân bằng. 

Việc tối ưu hóa xuất phát từ việc chúng ta chỉ cần nhớ lần xuất hiện sớm nhất của mỗi tổng tiền tố. Khi chúng ta thấy lại tổng tiền tố giống nhau, khoảng cách giữa các lần xuất hiện sẽ mang lại một phân đoạn cân bằng hợp lệ và chúng ta tối đa hóa nó bằng cách giữ lại lần xuất hiện đầu tiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) hoặc O(n) | Quá chậm | 
| Băm tổng tiền tố | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Chuyển đổi chuỗi thành số dư hiện hành trong đó`U`đóng góp +1 và`D`đóng góp -1. Chúng tôi theo dõi tổng tiền tố đại diện cho số dư này ở mọi vị trí. Điều này cho phép chúng ta phát hiện sự bình đẳng của`U`Và`D`bằng cách kiểm tra xem tổng có trở về giá trị trước đó hay không. 
2. Khởi tạo một từ điển lưu trữ chỉ mục sớm nhất nơi mỗi giá trị tổng tiền tố xuất hiện. Chúng tôi cũng lưu trữ tổng tiền tố 0 tại chỉ mục -1 trước khi bắt đầu xử lý. Điều này xử lý các phân đoạn bắt đầu từ chỉ mục 0 một cách chính xác, vì tiền tố cân bằng ngay từ đầu sẽ khớp với trạng thái cơ sở này. 
3. Lặp lại chuỗi, cập nhật tổng tiền tố khi chúng tôi thực hiện. Tại mỗi vị trí, chúng tôi kiểm tra xem tổng tiền tố này đã được nhìn thấy trước đó hay chưa. 
4. Nếu tổng tiền tố đã được nhìn thấy, mảng con giữa chỉ mục trước đó và chỉ mục hiện tại được cân bằng. Chúng tôi tính toán độ dài của nó và cập nhật câu trả lời tối đa. 
5. Nếu tổng tiền tố chưa được nhìn thấy, chúng tôi ghi lại chỉ mục của nó là lần xuất hiện đầu tiên. Chúng tôi không bao giờ ghi đè các lần xuất hiện trước đó vì các chỉ mục trước đó tạo ra các phân đoạn hợp lệ dài hơn khi khớp sau đó. 
6. Sau khi xử lý toàn bộ chuỗi, xuất ra độ dài tối đa tìm được. 

### Tại sao nó hoạt động 

Tổng tiền tố tại vị trí i mã hóa sự khác biệt giữa số lượng`U`Và`D`cho đến thời điểm đó. Hai vị trí có tổng tiền tố giống nhau ngụ ý rằng đóng góp ròng giữa chúng bằng 0, nghĩa là số lượng bằng nhau`U`Và`D`. Việc lưu trữ lần xuất hiện sớm nhất đảm bảo rằng mỗi lần lặp lại sau đó của cùng một số dư sẽ tạo ra đoạn dài nhất có thể kết thúc tại vị trí đó. Không có phân đoạn cân bằng nào bị bỏ sót vì mọi phân đoạn hợp lệ đều tương ứng với một số cặp tổng tiền tố bằng nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    
    for _ in range(t):
        n = int(input())
        s = input().strip()
        
        first_pos = {0: -1}
        pref = 0
        best = 0
        
        for i, ch in enumerate(s):
            if ch == 'U':
                pref += 1
            else:
                pref -= 1
            
            if pref in first_pos:
                best = max(best, i - first_pos[pref])
            else:
                first_pos[pref] = i
        
        out.append(str(best))
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Cấu trúc cốt lõi xoay quanh việc duy trì sự cân bằng tiền tố trong khi quét từ trái sang phải. Từ điển`first_pos`lưu trữ chỉ số sớm nhất mà mỗi số dư xuất hiện. Việc khởi tạo với`{0: -1}`rất quan trọng vì nó cho phép tiền tố kết thúc tại chỉ mục`i`với số dư ròng bằng 0 để tạo ra một đoạn có chiều dài một cách chính xác`i + 1`. 

Quy tắc cập nhật cho`pref`chuyển đổi luồng ký tự thành tín hiệu số, đây là phép biến đổi trung tâm giúp đơn giản hóa vấn đề. 

các`best`biến theo dõi khoảng cách tối đa giữa các tổng tiền tố lặp lại, tương ứng trực tiếp với phân đoạn hợp lệ dài nhất. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp đơn giản: 

Chuỗi đầu vào:`UUDD`Chúng tôi theo dõi tổng tiền tố và lần xuất hiện đầu tiên. 

| tôi | char | tiền tố | cập nhật bản đồ lần đầu tiên | tốt nhất | 
| --- | --- | --- | --- | --- | 
| -1 | - | 0 | {0: -1} | 0 | 
| 0 | Bạn | 1 | {0:-1, 1:0} | 0 | 
| 1 | Bạn | 2 | {0:-1, 1:0, 2:1} | 0 | 
| 2 | D | 1 | khớp ở 0 cho độ dài 2 | 2 | 
| 3 | D | 0 | khớp ở -1 cho độ dài 4 | 4 | 

Câu trả lời cuối cùng là 4, cho thấy toàn bộ chuỗi được cân bằng. 

Bây giờ hãy xem xét`UDUUDD`. 

| tôi | char | tiền tố | lần xuất hiện đầu tiên | tốt nhất | 
| --- | --- | --- | --- | --- | 
| 0 | Bạn | 1 | 1:0 | 0 | 
| 1 | D | 0 | 0:-1 tồn tại, cập nhật tốt nhất=2 | 2 | 
| 2 | Bạn | 1 | nhìn thấy ở 0 cho 2 | 2 | 
| 3 | Bạn | 2 | 2:3 | 2 | 
| 4 | D | 1 | nhìn thấy ở 0 cho 4 | 4 | 
| 5 | D | 0 | nhìn thấy ở -1 cho 6 | 6 | 

Điều này cho thấy các giá trị tiền tố lặp lại mở khóa nhiều phân đoạn hợp lệ như thế nào và tại sao việc giữ các vị trí sớm nhất là điều cần thiết để tối đa hóa độ dài. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần, các thao tác từ điển trung bình là O(1) | 
| Không gian | O(n) | Trong trường hợp xấu nhất, tất cả các tổng tiền tố đều khác biệt | 

Tổng kích thước đầu vào trên các trường hợp thử nghiệm bị giới hạn, do đó, việc quét tuyến tính trên mỗi trường hợp thử nghiệm là đủ để duy trì trong giới hạn. Cấu trúc băm chỉ lưu trữ tối đa n trạng thái tiền tố riêng biệt cho mỗi trường hợp thử nghiệm, phù hợp thoải mái với các hạn chế về bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def solve_io(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue() if False else run_solver(inp)

def run_solver(inp: str) -> str:
    import sys
    input = sys.stdin.readline
    t = int(input())
    res = []
    for _ in range(t):
        n = int(input())
        s = input().strip()
        first_pos = {0: -1}
        pref = 0
        best = 0
        for i, ch in enumerate(s):
            pref += 1 if ch == 'U' else -1
            if pref in first_pos:
                best = max(best, i - first_pos[pref])
            else:
                first_pos[pref] = i
        res.append(str(best))
    return "\n".join(res)

# provided sample (as intended individual cases)
assert run_solver("1\n4\nUDUD\n") == "4"
assert run_solver("1\n6\nUUUDDD\n") == "6"

# all same
assert run_solver("1\n5\nUUUUU\n") == "0"

# already balanced full string
assert run_solver("1\n8\nUDUDUDUD\n") == "8"

# no full balance but internal segment exists
assert run_solver("1\n5\nUUDUD\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| UUUUU | 0 | không tồn tại phân đoạn hợp lệ | 
| UDUD | 4 | cân bằng toàn chuỗi | 
| UUDUD | 4 | phân đoạn tốt nhất là nội bộ, không phải tiền tố | 

## Vỏ cạnh 

Khi chuỗi chỉ chứa một loại ký tự, mỗi tổng tiền tố hoàn toàn đơn điệu và không bao giờ lặp lại. Thuật toán vẫn khởi tạo từ điển với`{0: -1}`, nhưng không có tiền tố tương lai nào lại bằng 0, vì vậy`best`vẫn bằng không trong suốt. Điều này xử lý chính xác các đầu vào như`DDDDD`, trong đó không có phân đoạn hợp lệ tồn tại. 

Khi toàn bộ chuỗi được cân bằng, chẳng hạn như`UDUD`, tổng tiền tố trở về 0 ở chỉ mục cuối cùng. Vì ban đầu số 0 được lưu trữ ở chỉ mục -1 nên phân đoạn được tính toán sẽ trải rộng trên toàn bộ mảng. Thuật toán nắm bắt điều này một cách tự nhiên mà không cần cách viết đặc biệt. 

Khi các phân đoạn cân bằng tồn tại nhưng không được căn chỉnh tiền tố, chẳng hạn như`UUDUD`, tổng tiền tố lặp lại sẽ xuất hiện ở giữa quá trình quét. Mỗi lần lặp lại tạo thành một phân đoạn ứng cử viên một cách chính xác và chỉ mục được lưu trữ sớm nhất đảm bảo mở rộng tối đa cho giá trị tiền tố đó, đảm bảo tìm thấy phân đoạn dài nhất ngay cả khi nó bắt đầu và kết thúc ở giữa chuỗi.
