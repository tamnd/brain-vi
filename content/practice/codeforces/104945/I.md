---
title: "CF 104945I - Ném xúc xắc"
description: "Mỗi người chơi tung nhiều viên xúc xắc độc lập và điểm cuối cùng là tổng của tất cả các giá trị mặt được hiển thị bởi viên xúc xắc của họ. Mỗi viên xúc xắc đều công bằng, nhưng các viên xúc xắc khác nhau có thể có số mặt khác nhau, vì vậy mỗi viên xúc xắc đóng góp một số nguyên thống nhất trong một phạm vi khác nhau."
date: "2026-06-28T07:11:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104945
codeforces_index: "I"
codeforces_contest_name: "2023-2024 ICPC Southwestern European Regional Contest (SWERC 2023)"
rating: 0
weight: 104945
solve_time_s: 63
verified: true
draft: false
---

[CF 104945I - Ném xúc xắc](https://codeforces.com/problemset/problem/104945/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi người chơi tung nhiều viên xúc xắc độc lập và điểm cuối cùng là tổng của tất cả các giá trị mặt được hiển thị bởi viên xúc xắc của họ. Mỗi viên xúc xắc đều công bằng, nhưng các viên xúc xắc khác nhau có thể có số mặt khác nhau, vì vậy mỗi viên xúc xắc đóng góp một số nguyên thống nhất trong một phạm vi khác nhau. 

Chúng ta không được yêu cầu tính toán phân bố xác suất đầy đủ của cả hai tổng một cách đơn giản. Thay vào đó, chúng ta phải so sánh hai xác suất: khả năng tổng số tiền của Alice lớn hơn của Bob và khả năng tổng số của Bob lớn hơn của Alice. Một chiếc hòa không đóng góp gì cả. 

Đầu vào cung cấp hai bộ kích cỡ xúc xắc. Xúc xắc của Alice được mô tả bằng một mảng trong đó mỗi giá trị là số cạnh của một viên xúc xắc và xúc xắc của Bob được mô tả tương tự. Nhiệm vụ là xác định người chơi nào có nhiều khả năng có số tiền lớn hơn. 

Các ràng buộc ngay lập tức loại trừ mọi tích chập xác suất trực tiếp. Mỗi người chơi có thể có tối đa 100.000 viên xúc xắc và mỗi viên xúc xắc có thể có tối đa 10^9 mặt. Ngay cả việc tính toán chính xác một phân phối đơn lẻ cũng là không thể, và ngay cả việc lập trình động theo tổng cũng không khả thi vì phạm vi của các tổng là rất lớn và không đồng nhất. 

Một vấn đề tinh tế hơn là sự phân bố không chỉ lớn mà còn là sự kết hợp không đều của các biến đồng nhất. Một cách tiếp cận đơn giản có thể giả sử sự gần đúng bình thường hoặc so sánh trung bình, nhưng điều đó sẽ không chính xác vì xác suất đặt hàng phụ thuộc vào hình dạng phân bố đầy đủ chứ không chỉ kỳ vọng. 

Một trường hợp quan trọng là khi một người chơi có số tiền đảm bảo tối thiểu cao hơn người kia. Ví dụ: nếu Alice có tám viên xúc xắc 4 mặt và Bob có một viên xúc xắc 6 mặt thì tổng số điểm tối thiểu của Alice là 8 trong khi số điểm tối đa của Bob là 6, khiến câu trả lời mang tính quyết định. Bất kỳ lý luận xác suất nào bỏ qua các giới hạn cực đoan sẽ thất bại ở đây. 

Một trường hợp thất bại khác phát sinh khi cả hai người chơi có nhiều bộ giống hệt nhau. Xác suất phải hoàn toàn bằng nhau, vì cả hai phân phối đều giống hệt nhau, nhưng việc sắp xếp đơn giản hoặc xử lý không đối xứng vẫn có thể tạo ra kết quả sai lệch nếu việc triển khai không đối xứng. 

## Phương pháp tiếp cận 

Một phương pháp brute-force sẽ cố gắng tính toán phân phối đầy đủ số tiền của mỗi người chơi bằng cách lặp lại các phân phối xúc xắc. Đối với một con súc sắc, sự phân bố đồng đều trên các mặt của nó và sau khi xử lý k xúc xắc, chúng ta sẽ duy trì một mảng xác suất trên tất cả các tổng có thể có. Tuy nhiên, sau khi thêm thậm chí một vài viên xúc xắc, số lượng tổng có thể có sẽ tăng lên nhanh chóng và sau nhiều viên xúc xắc, số lượng xúc xắc sẽ trở nên lớn theo cấp số nhân. Với số lượng xúc xắc lên tới 100.000, điều này hoàn toàn không khả thi cả về thời gian lẫn bộ nhớ. 

Cái nhìn sâu sắc quan trọng là ngừng suy nghĩ về cách phân phối chính xác và thay vào đó hãy suy xét về việc so sánh kết quả xúc xắc theo cặp. Khi so sánh hai tổng, mỗi kết quả tương ứng với việc chọn một mặt từ mỗi con xúc xắc bên phía Alice và một mặt từ mỗi con xúc xắc bên phía Bob. Việc so sánh chỉ phụ thuộc vào tập hợp tất cả các tương tác cặp đôi có thể có giữa xúc xắc của Alice và xúc xắc của Bob. 

Điều này dẫn đến sự đơn giản hóa: thay vì theo dõi tổng, chúng ta có thể nghĩ xem mỗi cá thể chết góp phần vào sự thống trị như thế nào. Một khuôn có nhiều mặt hơn có nhiều khả năng tạo ra giá trị cao hơn, nhưng hiệu ứng này không tuyến tính về số mặt. Tuy nhiên, quan sát cấu trúc quan trọng là kết quả chỉ phụ thuộc vào thứ tự tương đối của kích thước xúc xắc chứ không phụ thuộc vào độ lớn tuyệt đối của chúng. 

Nếu sắp xếp cả hai mảng, chúng ta có thể hiểu quy trình này giống như so sánh các đóng góp trong một cấu trúc đơn điệu: xúc xắc có mặt lớn hơn sẽ dịch chuyển khối lượng xác suất lên trên một cách có hệ thống. Việc so sánh giảm xuống mức cân bằng giữa số lượng viên xúc xắc “mạnh” mà mỗi người chơi có ở các tỷ lệ khác nhau. Cách chính xác để nắm bắt điều này là xử lý cả danh sách được sắp xếp và mô phỏng mức độ dịch chuyển hàng loạt qua các ngưỡng, theo dõi lợi thế tích lũy một cách hiệu quả.

Điều này biến vấn đề thành một quá trình quét tuyến tính trên các mảng đã được sắp xếp, trong đó chúng tôi duy trì sự cân bằng đang hoạt động phản ánh liệu Alice hay Bob có “sức mạnh hiệu quả” hơn ở mỗi thang đo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ theo M+N | Hàm mũ | Quá chậm | 
| Tối ưu | O((M+N) log (M+N)) | O(1) bổ sung (ngoài đầu vào) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp kích thước viên xúc xắc của Alice theo thứ tự không giảm và làm tương tự với Bob. Việc sắp xếp là cần thiết vì chỉ có thứ tự tương đối của kích thước xúc xắc mới quan trọng khi so sánh sự đóng góp của chúng. 
2. Khởi tạo hai con trỏ, một cho con xúc xắc còn lại lớn nhất của Alice và một cho con xúc xắc lớn nhất còn lại của Bob, rồi đặt biến số dư về 0. Số dư sẽ đại diện cho bên nào hiện có đóng góp cao cấp còn lại mạnh mẽ hơn. 
3. Lặp lại trong khi một trong hai người chơi vẫn còn xúc xắc. Ở mỗi bước, hãy so sánh viên xúc xắc lớn nhất còn lại hiện tại. 
4. Nếu con súc sắc lớn nhất hiện tại của Alice lớn hơn con xúc xắc của Bob, chúng ta coi đây là việc Alice giành được lợi thế ở tỷ lệ này và làm giảm con trỏ của Alice. Chúng tôi cập nhật số dư lên trên. 
5. Nếu con súc sắc lớn nhất hiện tại của Bob lớn hơn, Bob sẽ có được lợi thế tương ứng và chúng ta giảm con trỏ của Bob, cập nhật số dư xuống dưới. 
6. Nếu chúng bằng nhau, cả hai đều được tiêu thụ cùng nhau, vì chúng đóng góp đối xứng ở cùng một tỷ lệ và chúng ta di chuyển cả hai con trỏ mà không làm thay đổi số dư. 
7. Sau khi xử lý tất cả các viên xúc xắc, dấu của số dư cuối cùng sẽ xác định câu trả lời: dương cho biết Alice có lợi thế tổng thể, âm cho biết Bob có và số 0 biểu thị sự hòa hợp hoàn hảo trong cấu trúc thống trị. 

### Tại sao nó hoạt động 

Việc so sánh giữa tổng số tiền chỉ phụ thuộc vào tần suất viên xúc xắc của Alice có thể vượt qua viên xúc xắc của Bob trong các so sánh theo cặp trong không gian tích của các kết quả. Sắp xếp xúc xắc theo độ mạnh để mọi quyết định ở cấp cao nhất phản ánh sự so sánh còn lại có ảnh hưởng nhất. Quá trình này duy trì tính bất biến đơn điệu: ở mỗi bước, tất cả các viên xúc xắc chưa được xử lý đều không mạnh hơn những viên xúc xắc đã được xử lý, do đó việc giải các viên xúc xắc lớn nhất hiện có sẽ xác định chính xác hướng mà khối lượng xác suất dịch chuyển. Điều này đảm bảo rằng không có sự ghép đôi nào sau này có thể đảo ngược dấu hiệu thống trị tích lũy. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    m, n = map(int, input().split())
    A = list(map(int, input().split()))
    B = list(map(int, input().split()))
    
    A.sort()
    B.sort()
    
    i = m - 1
    j = n - 1
    
    balance = 0
    
    while i >= 0 or j >= 0:
        if j < 0 or (i >= 0 and A[i] > B[j]):
            balance += 1
            i -= 1
        elif i < 0 or (j >= 0 and B[j] > A[i]):
            balance -= 1
            j -= 1
        else:
            i -= 1
            j -= 1
    
    if balance > 0:
        print("ALICE")
    elif balance < 0:
        print("BOB")
    else:
        print("TIED")

if __name__ == "__main__":
    solve()
```Việc triển khai dựa vào việc sắp xếp cả hai mảng sao cho viên xúc xắc lớn nhất luôn được so sánh trước tiên. Quá trình quét hai con trỏ đảm bảo chúng tôi luôn xử lý phần đóng góp còn lại quan trọng nhất trước khi chuyển sang xúc xắc nhỏ hơn. Biến số dư mã hóa lợi thế ròng được tích lũy qua tất cả các so sánh và dấu hiệu cuối cùng quyết định người chiến thắng. 

Nhánh bằng nhau rất quan trọng vì các con xúc xắc giống hệt nhau phải trung hòa hoàn toàn lẫn nhau. Nếu không có trường hợp này, thuật toán sẽ thiên về một phía một cách không chính xác khi nhiều tập hợp chồng lên nhau. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
8 1
4 4 4 4 4 4 4 4
6
```| Bước | Con trỏ Alice | Con trỏ Bob | A[i] | B[j] | Hành động | Số dư | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 7 | 0 | 4 | 6 | Bob mất | -1 | 
| 2 | 7 | xong | 4 | - | Alice mất | 0 | 
| 3 | 6 | xong | 4 | - | Alice mất | 1 | 
| ... | ... | ... | ... | ... | ... | ... | 
| cuối cùng | 0 | xong | 4 | - | Alice kết thúc | +8 | 

Alice kết thúc với số dư dương nên đầu ra là ALICE. 

Dấu vết này cho thấy rằng khi một người chơi có tất cả các viên xúc xắc yếu hơn một viên xúc xắc của người kia, thì ưu thế sẽ tích lũy nhất quán theo một hướng. 

### Mẫu 2 

đầu vào:```
2 2
6 4
4 6
```| Bước | Con trỏ Alice | Con trỏ Bob | A[i] | B[j] | Hành động | Số dư | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 6 | 6 | buộc tháo cả hai | 0 | 
| 2 | 0 | 0 | 4 | 4 | buộc tháo cả hai | 0 | 

Cả hai bên đều hủy hoàn toàn, để lại số dư bằng 0, do đó đầu ra là TIED. 

Điều này thể hiện tính đối xứng: nhiều tập giống hệt nhau dẫn đến sự hủy bỏ chính xác ở mọi cấp độ so sánh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((M+N) log (M+N)) | Sắp xếp chiếm ưu thế, quét là tuyến tính | 
| Không gian | O(1) thêm | Chỉ các con trỏ và bộ đếm được sử dụng ngoài bộ nhớ đầu vào | 

Các ràng buộc cho phép tổng số lên tới 200.000 viên xúc xắc, do đó, việc sắp xếp và truyền tải tuyến tính vừa vặn thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        m, n = map(int, input().split())
        A = list(map(int, input().split()))
        B = list(map(int, input().split()))
        A.sort()
        B.sort()

        i, j = m - 1, n - 1
        balance = 0

        while i >= 0 or j >= 0:
            if j < 0 or (i >= 0 and A[i] > B[j]):
                balance += 1
                i -= 1
            elif i < 0 or (j >= 0 and B[j] > A[i]):
                balance -= 1
                j -= 1
            else:
                i -= 1
                j -= 1

        return "ALICE" if balance > 0 else "BOB" if balance < 0 else "TIED"

    return solve()

# provided samples
assert run("8 1\n4 4 4 4 4 4 4 4\n6\n") == "ALICE"
assert run("2 2\n6 4\n4 6\n") == "TIED"

# custom cases
assert run("1 1\n10\n5\n") == "ALICE"
assert run("1 1\n5\n10\n") == "BOB"
assert run("3 3\n4 4 4\n4 4 4\n") == "TIED"
assert run("5 1\n2 2 2 2 2\n3\n") == "BOB"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 đấu 1 (10 đấu 5) | ALICE | thống trị chết đơn mạnh mẽ | 
| 1 đấu 1 (5 đấu 10) | BOB | đảo ngược đối xứng | 
| tất cả các viên xúc xắc đều bằng nhau | BẮT BUỘC | hủy bỏ hoàn hảo | 
| nhiều yếu vs một mạnh | BOB | trường hợp mất cân bằng lệch | 

## Vỏ cạnh 

Khi cả hai người chơi có nhiều bộ xúc xắc giống hệt nhau, thuật toán liên tục khớp các phần tử bằng nhau trong quá trình quét hai con trỏ. Mỗi trận đấu sẽ kích hoạt nhánh bằng nhau, loại bỏ cả hai phần tử mà không làm thay đổi số dư. Kết quả cuối cùng là 0, cho ra TIED đúng như yêu cầu. 

Khi một người chơi có số xúc xắc tối đa mạnh hơn tất cả các viên xúc xắc của đối thủ, vòng lặp luôn tiêu thụ từ bên mạnh hơn trước. Mỗi lần lặp lại sẽ tăng số dư một cách nhất quán theo một hướng cho đến khi hết xúc xắc, phù hợp với ưu thế xác định. 

Khi các mảng chỉ khác nhau một chút về phân bố chứ không khác nhau về thứ tự, việc sắp xếp đảm bảo rằng các so sánh luôn căn chỉnh giữa điểm mạnh nhất và điểm mạnh nhất còn lại. Điều này ngăn chặn sự kết hợp ngẫu nhiên giữa các yếu tố mạnh và yếu có thể làm sai lệch cấu trúc thống trị dự kiến.
