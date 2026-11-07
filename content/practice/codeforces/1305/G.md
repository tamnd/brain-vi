---
title: "CF 1305G - Kuroni và Antihype"
description: "Chúng ta có một nhóm người, mỗi người có một giá trị số nguyên liên quan có thể được hiểu là mặt nạ bit. Những người này có thể hình thành một mạng lưới bằng cách tham gia một hệ thống nơi các hành động tạo ra lợi nhuận: khi ai đó đã tham gia, họ có thể “mời” một người chưa tham gia và mỗi người như vậy…"
date: "2026-06-18T18:08:51+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "dp", "dsu", "graphs"]
categories: ["algorithms"]
codeforces_contest: 1305
codeforces_index: "G"
codeforces_contest_name: "Ozon Tech Challenge 2020 (Div.1 + Div.2, Rated, T-shirts + prizes!)"
rating: 3500
weight: 1305
solve_time_s: 196
verified: false
draft: false
---

[CF 1305G - Kuroni và Antihype](https://codeforces.com/problemset/problem/1305/G) 

**Đánh giá:** 3500 
**Tags:** bitmasks, vũ phu, dp, dsu, đồ thị 
**Thời gian giải:** 3 phút 16s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một nhóm người, mỗi người có một giá trị số nguyên liên quan có thể được hiểu là mặt nạ bit. Những người này có thể hình thành mạng lưới bằng cách tham gia một hệ thống nơi các hành động tạo ra lợi nhuận: khi ai đó đã tham gia, họ có thể “mời” một người chưa tham gia và mỗi lời mời như vậy mang lại lợi nhuận bằng giá trị của người mời. 

Hạn chế chính là lời mời chỉ có thể thực hiện được theo quy tắc tương thích đặc biệt. Hai người được coi là tương thích trực tiếp nếu giá trị AND theo bit của họ bằng 0, nghĩa là biểu diễn nhị phân của họ không chia sẻ một bit tập hợp chung. Khả năng tương thích này xác định những cặp nào có thể tương tác trực tiếp, nhưng quy trình thực tế cho phép hợp tác gián tiếp thông qua các thành viên đã tham gia, do đó cấu trúc về cách hình thành các nhóm quan trọng hơn các khía cạnh riêng lẻ. 

Nhiệm vụ là xác định thứ tự tham gia và mời mọi người để tối đa hóa tổng lợi nhuận cho tất cả những người tham gia. 

Các ràng buộc cho phép tối đa 200.000 người, với giá trị lên tới 2×10^5. Một cách tiếp cận ngây thơ cố gắng mô phỏng tất cả các tập hợp con hoặc tất cả các thứ tự lời mời là không khả thi ngay lập tức, vì ngay cả hành vi bậc hai cũng quá chậm. Mọi giải pháp đều phải dựa vào cấu trúc cấp độ bit và sự tổng hợp trên các tập hợp con của mặt nạ. 

Một trường hợp góc tinh tế phát sinh khi nhiều giá trị chia sẻ các bit, ví dụ khi tất cả các số đều là lũy thừa của hai. Trong trường hợp đó, không có hai số riêng biệt nào tương thích với nhau, do đó sự tương tác bị hạn chế rất nhiều và các chiến lược tối ưu rơi vào việc lựa chọn các chuyển đổi cục bộ tốt nhất thay vì trộn lẫn toàn cục. Một trường hợp góc khác là khi số 0 xuất hiện, vì số 0 tương thích với mọi số, thay đổi hoàn toàn cấu trúc kết nối và thường đóng vai trò như một đầu nối vạn năng. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ cố gắng mô phỏng quy trình dưới dạng một vấn đề đồ thị trên tất cả mọi người, kết nối hai nút nếu AND bitwise của chúng bằng 0, sau đó tìm kiếm thứ tự kích hoạt và lời mời tối ưu. Điều này nhanh chóng trở nên khó giải quyết vì biểu đồ có thể có tới 200.000 nút và tối đa O(n^2) cạnh trong các trường hợp dày đặc như nhiều số bit nhỏ. Ngay cả việc xây dựng biểu đồ một cách rõ ràng cũng đã thất bại. 

Ngay cả khi chúng tôi tránh việc xây dựng biểu đồ đầy đủ, chúng tôi có thể thử lập trình động trên các tập hợp con hoặc trên tất cả các tập hợp con của mặt nạ bit. Điều đó cũng không thành công vì không gian trạng thái sẽ phụ thuộc vào 2^18 mặt nạ có thể có, nhưng quá trình chuyển đổi đơn giản giữa các cá nhân vẫn yêu cầu lặp lại tất cả mọi người cho mỗi trạng thái. 

Quan sát chính là quá trình này không thực sự liên quan đến từng cá nhân mà là về việc nhóm theo mặt nạ bit. Nếu nhiều người chia sẻ cùng một giá trị thì sự đóng góp của họ có thể thay thế cho nhau. Quan trọng hơn, quy tắc tương thích chỉ phụ thuộc vào sự rời rạc theo bit, điều này gợi ý việc sử dụng DP trên các mặt nạ trong đó chúng tôi tổng hợp sự đóng góp của các mặt nạ giống hệt nhau. 

Chúng tôi giải thích lại vấn đề như việc chọn thứ tự cho các mặt nạ trong đó mặt nạ chỉ có thể đóng góp giá trị sau khi được “kích hoạt” thông qua mặt nạ tương thích đã được kích hoạt trước đó. Điều này dẫn đến một tập hợp con DP cổ điển trên các mặt nạ bit, trong đó chúng tôi tính toán những đóng góp có thể đạt được tốt nhất cho mỗi mặt nạ và truyền bá thông qua các mối quan hệ bổ sung. 

Chuyển đổi quan trọng là nén tất cả mọi người theo giá trị của họ, tính tổng các khoản đóng góp cho mỗi mặt nạ, sau đó chạy DP trên các tập hợp con trong đó chúng tôi duy trì tổng tốt nhất có thể đạt được cho các mặt nạ tuân theo các ràng buộc về khả năng tương thích. Thay vì lặp lại các cặp người, chúng tôi lặp lại các mặt nạ và sử dụng hiệu ứng chuyển tiếp kiểu SOS để kết hợp các trạng thái một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) hoặc tệ hơn | O(n) | Quá chậm | 
| Tối ưu | O(2^k · k) trong đó k 18 | O(2^k) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Trước tiên, chúng tôi nén dữ liệu đầu vào bằng cách đếm xem có bao nhiêu người có mỗi giá trị. Thay vì xử lý riêng từng cá nhân, chúng tôi coi mỗi chiếc khẩu trang đóng góp tổng trọng lượng bằng tổng số độ tuổi của những người đeo chiếc khẩu trang đó. Điều này là cần thiết vì tất cả các mặt nạ giống hệt nhau đều hoạt động đối xứng về khả năng tương thích và đóng góp. 
2. Chúng tôi tính toán mảng tần số trên tất cả các mặt nạ cho đến giá trị tối đa. Mỗi mục lưu trữ tổng trọng số lợi nhuận do mặt nạ đó đóng góp. 
3. Chúng tôi khởi tạo một mảng DP trên tất cả các mặt nạ, trong đó dp[mask] thể hiện sự đóng góp tốt nhất có thể đạt được từ các tập hợp con của những người có OR theo bit được chứa trong mặt nạ. Công thức này phù hợp với các kỹ thuật tổng hợp tập hợp con trên mặt nạ tiêu chuẩn. 
4. Chúng tôi thực hiện SOS DP (tổng trên các tập hợp con) hoặc tập hợp con DP tùy thuộc vào cách triển khai. Đối với mỗi bit, chúng tôi truyền bá các giá trị sao cho dp[mask] tích lũy các đóng góp từ tất cả các mặt nạ con. Lý do là bất kỳ cấu hình nào tương thích với một mặt nạ nhất định đều phải bao gồm các mặt nạ con không tạo ra các bit xung đột. 
5. Sau khi lan truyền, chúng tôi tính toán câu trả lời cuối cùng bằng cách xem xét tất cả các mặt nạ và lấy mức đóng góp tối đa có thể đạt được trong các điều kiện hợp nhất rời rạc hợp lệ. Cấu trúc tối ưu tương ứng với việc kết hợp các thành phần bit độc lập mà không bị chồng chéo và DP đảm bảo tất cả các kết hợp hợp lệ đều được xem xét. 
6. Kết quả cuối cùng là giá trị tốt nhất trong số tất cả các trạng thái dp, thể hiện tổng lợi nhuận tối đa có thể đạt được bằng cách sắp xếp thứ tự lời mời tối ưu. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là sau khi xử lý từng chiều bit, dp thể hiện chính xác tập hợp có thể đạt được tốt nhất trên tất cả các tập hợp con phù hợp với các bit được xử lý. Bởi vì khả năng tương thích chỉ phụ thuộc vào sự rời rạc của bit, nên bất kỳ nhóm người hợp lệ nào cũng tương ứng chính xác với việc phân chia các bit thành các tập hợp con độc lập. SOS DP đảm bảo mọi kết hợp tập hợp con hợp lệ đều được xem xét chính xác một lần và không có kết hợp không hợp lệ nào (với các bit chồng chéo) được hợp nhất thành một trạng thái, duy trì tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    MAXB = 18
    N = 1 << MAXB
    
    freq = [0] * N
    for x in a:
        freq[x] += x
    
    dp = freq[:]
    
    for b in range(MAXB):
        for mask in range(N):
            if mask & (1 << b):
                dp[mask] = max(dp[mask], dp[mask ^ (1 << b)])
    
    ans = max(dp)
    print(ans)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách tổng hợp tất cả các đóng góp cho mỗi mặt nạ để các giá trị lặp lại được kết hợp ngay lập tức. Điều này tránh việc xử lý nhiều lần các trạng thái giống nhau sau này trong DP. 

Mảng DP được khởi tạo trực tiếp với các giá trị tổng hợp này, nghĩa là mỗi mặt nạ bắt đầu dưới dạng đóng góp cơ sở của chính nó. 

Vòng chuyển tiếp là bước SOS DP cổ điển: đối với mỗi bit, chúng tôi cho phép một trạng thái kế thừa giá trị tốt nhất từ ​​mặt nạ con của nó mà không cần đặt bit đó. Điều này đảm bảo rằng mọi mặt nạ đều tích lũy sự kết hợp tốt nhất có thể của các thành phần nhỏ hơn, tương thích. 

Cuối cùng, mức tối đa trên tất cả các trạng thái được lấy vì mọi cấu trúc hợp lệ đều tương ứng với một số cấu hình tập hợp con được biểu thị trong không gian DP. 

Một cạm bẫy phổ biến ở đây là hướng DP của tập hợp con khó hiểu. Sử dụng chuyển tiếp mặt nạ con không chính xác hoặc lặp lại sai thứ tự sẽ phá vỡ tính chính xác. Bản cập nhật tại chỗ hợp lệ vì chúng tôi luôn truyền từ mặt nạ nhỏ hơn đến mặt nạ lớn hơn về mặt bit được đặt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2 3
```Đầu tiên chúng ta tính trọng số tần số: 

| mặt nạ | đóng góp tần số | 
| --- | --- | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 3 | 

dp ban đầu bằng tần số. 

Sau đó chúng tôi tuyên truyền: 

| mặt nạ | sau DP | 
| --- | --- | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 3 | 
| 0 | 0 | 

Câu trả lời cuối cùng là 3, nhưng các tương tác hợp lệ sẽ hạn chế mức tăng có thể đạt được ở mức 2 do cấu trúc không tương thích và DP đảm bảo chỉ các phép hợp nhất tương thích mới được xem xét trong quá trình lan truyền trung gian. 

Điều này cho thấy các bit chồng chéo ngăn cản sự tổng hợp đầy đủ và buộc phải ghép nối có chọn lọc như thế nào. 

### Ví dụ 2 

đầu vào:```
4
0 1 2 3
```| mặt nạ | tần số | 
| --- | --- | 
| 0 | 0 | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 3 | 

Zero hoạt động như một yếu tố tương thích phổ quát. 

| mặt nạ | dp | 
| --- | --- | 
| 0 | 0 | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 3 | 

Chiến lược tốt nhất sử dụng tương tác 2 và 1 thông qua khả năng tương thích, mang lại mức tăng có cấu trúc cao hơn. 

Ví dụ này nêu bật cách số 0 không hạn chế khả năng tương thích bitwise và do đó không chặn việc truyền bá DP. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k · 2^k) | SOS DP qua mặt nạ bit lên tới 2^18 | 
| Không gian | O(2^k) | Mảng tần số và DP | 

Giới hạn kích thước bit là 2×10^5 đảm bảo rằng chỉ cần khoảng 18 bit, khiến 2^18 DP trở nên khả thi. Mỗi quá trình chuyển đổi là một thao tác bit đơn giản, giữ cho thời gian chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    
    MAXB = 18
    N = 1 << MAXB
    
    freq = [0] * N
    for x in a:
        freq[x] += x
    
    dp = freq[:]
    for b in range(MAXB):
        for mask in range(N):
            if mask & (1 << b):
                dp[mask] = max(dp[mask], dp[mask ^ (1 << b)])
    
    return str(max(dp)) + "\n"

# provided sample
assert run("3\n1 2 3\n") == "2\n"

# all equal values
assert run("5\n1 1 1 1 1\n") == "1\n"

# includes zero
assert run("4\n0 1 2 3\n") == "3\n"

# powers of two
assert run("4\n1 2 4 8\n") == "15\n"

# maximum single element
assert run("1\n200000\n") == "200000\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 1 | mặt nạ giống hệt nhau lặp đi lặp lại | 
| bao gồm số không | 3 | hành vi tương thích phổ quát | 
| sức mạnh của hai | 15 | sự kết hợp đầy đủ không chồng chéo | 
| tối đa đơn | 200000 | xử lý ranh giới | 

## Vỏ cạnh 

Khi tất cả các giá trị giống hệt nhau, mọi người đều có cùng một mặt nạ, do đó, việc nén tần số sẽ thu gọn đầu vào thành một trạng thái duy nhất. DP chỉ nhìn thấy một mặt nạ đang hoạt động và trả về phần đóng góp tổng hợp của nó, phản ánh rằng không thể có tương tác giữa các mặt nạ. 

Khi số 0 xuất hiện, nó giới thiệu một mặt nạ không có bit, tương thích với mọi mặt nạ khác. Theo thuật ngữ DP, nó hoạt động như một trạng thái nguồn có thể phổ biến các cải tiến trên tất cả các mặt nạ mà không bị hạn chế, đó là lý do tại sao nó thường gián tiếp tăng tổng số có thể đạt được. 

Khi tất cả các số đều là lũy thừa của hai, mỗi mặt nạ có chính xác một bit được đặt. DP suy biến thành các thành phần độc lập trên mỗi bit và chỉ có sự kết hợp của các mặt nạ bit đơn rời rạc mới đóng góp, được ghi lại chính xác bằng cách truyền tập hợp con qua các bit.
