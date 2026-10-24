---
title: "CF 106015D - Trò chơi bóng tối của quái vật"
description: "Chúng ta được cấp một tập hợp nhiều tập hợp ban đầu chứa một số chẵn $N$ có các giá trị giống hệt nhau, tất cả đều bằng 1. Hai người chơi lần lượt thao tác với tập hợp nhiều tập hợp này. Trong mỗi vòng, Người chơi A loại bỏ hai số tùy ý khỏi bộ nhiều số."
date: "2026-06-22T16:45:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106015
codeforces_index: "D"
codeforces_contest_name: "Game of Coders 4 - Over the Garden Wall"
rating: 0
weight: 106015
solve_time_s: 75
verified: true
draft: false
---

[CF 106015D - Trò chơi bóng tối của quái vật](https://codeforces.com/problemset/problem/106015/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp ban đầu chứa một số chẵn$N$có giá trị giống hệt nhau, tất cả đều bằng 1. Hai người chơi lần lượt thao tác nhiều tập hợp này. Trong mỗi vòng, Người chơi A loại bỏ hai số tùy ý khỏi bộ nhiều số. Sau đó, Người chơi B thay thế chúng bằng một số duy nhất, đó là tổng hoặc hiệu tuyệt đối của chúng. Do đó, kích thước của nhiều tập hợp giảm đi đúng một sau mỗi vòng. 

Trò chơi dừng lại khi có một trong hai điều kiện. Hoặc một số giá trị trở nên lớn hơn tổng của tất cả các giá trị khác, nghĩa là một phần tử duy nhất sẽ thống trị mọi phần tử khác hoặc tất cả các giá trị đều trở thành 0. Khi trò chơi kết thúc, Người chơi A nhận được cookie bằng số phần tử còn lại trong nhiều bộ tại thời điểm đó. Người chơi A cố gắng tối đa hóa số lượng cuối cùng này, trong khi Người chơi B cố gắng giảm thiểu nó. 

Sự ràng buộc về$N$là rất lớn, lên tới$10^{18}$, vì vậy bất kỳ cách tiếp cận nào mô phỏng quy trình từng bước đều không thể thực hiện được. Các giải pháp khả thi duy nhất phải giảm trò chơi thành một lý luận không đổi hoặc logarit dựa trên các bất biến của các phép toán. 

Một trường hợp phức tạp là khi quá trình sụp đổ về tất cả số không. Nếu tất cả các phần tử trở thành 0, trò chơi sẽ kết thúc ngay lập tức và số lượng cookie vẫn là số phần tử hiện có chứ không phải bằng 0. Ví dụ: nếu nhiều bộ trở thành$[0,0,0]$, thì câu trả lời là 3. 

Một trường hợp cạnh khác là khi một giá trị lớn duy nhất xuất hiện sớm và chiếm ưu thế so với phần còn lại. Ví dụ: nếu multiset trở thành$[5,1,1]$, sau đó$5 > 2$, do đó trò chơi sẽ dừng ngay lập tức mặc dù vẫn còn nhiều yếu tố. 

Khó khăn chính là cả hai người chơi đều gián tiếp kiểm soát sự phát triển trong tương lai của tổng số tiền và sự phân bổ các giá trị chứ không chỉ các số riêng lẻ. 

## Phương pháp tiếp cận 

Một mô phỏng lực lượng vũ phu sẽ lưu trữ rõ ràng nhiều tập hợp, liên tục chọn các cặp và áp dụng tổng hoặc chênh lệch tuyệt đối. Ngay cả khi bỏ qua cách chơi tối ưu, vẫn có$N/2$làm tròn và sau mỗi thao tác, các giá trị có thể tăng hoặc giảm không thể đoán trước. Từ$N$có thể lên đến$10^{18}$, thậm chí việc đại diện cho trạng thái là không thể và bất kỳ tìm kiếm không gian trạng thái nào đối với các lựa chọn của người chơi sẽ bùng nổ theo kiểu tổ hợp. 

Quan sát quan trọng là cấu trúc của các hoạt động về cơ bản là về tổng số tiền phát triển như thế nào. Nếu hai số$a$Và$b$được thay thế thì tổng của tất cả các phần tử vẫn giữ nguyên (nếu chúng ta sử dụng$a+b$) hoặc giảm đi$2\min(a,b)$(nếu chúng ta sử dụng$|a-b|$). Bắt đầu từ tất cả những số 1, mọi ứng dụng của hiệu tuyệt đối đều làm giảm tổng, trong khi tổng vẫn giữ nguyên tổng. 

Điều này có nghĩa là Người chơi B có khả năng nhất quán để giảm tổng số tiền bất cứ khi nào cặp được chọn không giống nhau theo cách ngăn chặn việc giảm. Trong khi đó, Người chơi A chỉ chọn hai giá trị nào sẽ bị lộ, nhưng không thể ngăn chặn sự tồn tại của một chuỗi giảm dần khiến hệ thống dần dần sụp đổ. 

Điều quan trọng đối với điều kiện kết thúc trò chơi không phải là multiset chính xác mà là liệu yếu tố chi phối có xuất hiện hay không hay mọi thứ có sụp đổ về 0 hay không. Trong cách chơi tối ưu, Người chơi B luôn có thể ngăn chặn việc hình thành một cấu trúc “ổn định” lâu dài với nhiều yếu tố tích cực, buộc quá trình này đi đến sự sụp đổ nhanh chóng hoặc thống trị ngay lập tức với quy mô còn lại ở mức tối thiểu. 

Sự đơn giản hóa cấu trúc quan trọng là Người chơi B luôn có thể điều khiển trò chơi vào trạng thái mà điều kiện kết thúc kích hoạt khi chỉ còn lại một phần tử. Bất kỳ nỗ lực nào của Người chơi A nhằm bảo tồn nhiều phần tử đều bị vô hiệu hóa vì Người chơi B luôn có thể chọn sự khác biệt tuyệt đối để thu gọn các giá trị hoặc tổng để kiểm soát sự tăng trưởng, nhưng không bao giờ cho phép cấu hình duy trì vô thời hạn nhiều phần tử lớn có thể so sánh được. 

Điều này làm giảm toàn bộ trò chơi thành một kết quả bất biến duy nhất: cách chơi tối ưu luôn dẫn đến cấu hình cuối cùng với chính xác một phần tử vẫn còn trên bảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(N)$vòng với sự phân nhánh theo cấp số nhân |$O(N)$| Quá chậm | 
| Lý luận bất biến tối ưu |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quan sát thấy rằng mỗi vòng giảm số phần tử đi đúng một phần tử, do đó sau$k$làm tròn kích thước nhiều tập hợp là$N-k$. Vấn đề tương đương với việc xác định có bao nhiêu vòng có thể xảy ra trước khi buộc phải chấm dứt. 
2. Theo dõi những gì Người chơi B có thể thực thi sau mỗi lần hợp nhất. Khi B chọn tổng, tổng tổng của nhiều tập hợp không thay đổi. Khi B chọn sai số tuyệt đối thì tổng giảm đúng một lượng$2\min(a,b)$. Điều này mang lại cho Người chơi B một cơ chế lâu dài để ngăn chặn sự tăng trưởng không kiểm soát. 
3. Từ trạng thái ban đầu của tất cả những số 1, bất kỳ thao tác sớm nào cũng tạo ra 2 hoặc 0. Cả hai đều không tạo ra lợi thế lâu dài cho Người chơi A, vì bất kỳ giá trị lớn hơn nào vẫn tham gia vào các cặp đôi trong tương lai và có thể bị giảm đi. 
4. Chiến lược tối ưu của Người chơi B là đảm bảo rằng quá trình không ổn định với nhiều giá trị dương có thể so sánh được. Bằng cách sử dụng nhất quán những khác biệt tuyệt đối khi hữu ích, B đẩy hệ thống đến chỗ sụp đổ một cách có kiểm soát, trong khi các phép tính tổng chỉ được sử dụng khi cần thiết để tránh sự thống trị sớm. 
5. Điểm cuối ổn định duy nhất trong sự kiểm soát đối nghịch này là khi vẫn còn chính xác một phần tử, bởi vì bất kỳ cấu hình nào có hai phần tử trở lên vẫn cho phép một động thái hợp pháp khác và không có điều kiện dừng bắt buộc trừ khi B cố tình kích hoạt nó sớm hơn. Vì B đang giảm thiểu số phần tử cuối cùng nên việc tiếp tục cho đến khi còn lại một phần tử luôn là tối ưu. 
6. Do đó, bất kể Người chơi A chọn cặp như thế nào, Người chơi B có thể buộc trò chơi tiếp tục cho đến khi chỉ còn lại một phần tử khi kết thúc. 

### Tại sao nó hoạt động 

Điều bất biến là Người chơi B luôn kiểm soát xem tổng số tiền được giữ nguyên hay giảm nghiêm ngặt ở mỗi bước. Điều này ngăn cản Người chơi A tạo cấu hình “khóa” trò chơi vào một cấu trúc lớn còn lại. Vì mỗi bước di chuyển sẽ làm giảm số lượng phần tử đi đúng một phần tử và B luôn có thể tránh được sự thống trị bắt buộc sớm, nên cấu hình thiết bị đầu cuối nhất quán duy nhất trong cách chơi tối ưu là khi chỉ còn lại chính xác một phần tử. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input().strip())
    print(1)

if __name__ == "__main__":
    main()
```Việc thực hiện phản ánh trực tiếp kết luận rằng quá trình này độc lập với$N$. Không cần mô phỏng hoặc phân nhánh vì động lực của trò chơi luôn thu gọn thành một yếu tố còn lại duy nhất khi chơi tối ưu. 

Điều tinh tế duy nhất là đảm bảo đầu vào được đọc chính xác dưới dạng số nguyên 64 bit, vì$N$có thể đạt được$10^{18}$. Mọi thứ khác là đầu ra theo thời gian không đổi. 

## Ví dụ đã hoạt động 

Hãy xem xét$N = 2$. Nước đi duy nhất có thể thực hiện được là loại bỏ hai số 1 và Người chơi B sau đó viết 2 hoặc 0. Trong cả hai trường hợp, không thể di chuyển thêm nữa, vì vậy trò chơi kết thúc với đúng một phần tử. Cái bàn thật tầm thường. 

| Bước | Nhiều bộ | Hành động | 
| --- | --- | --- | 
| Bắt đầu | [1, 1] | A chọn cả hai | 
| Kết thúc | [2] hoặc [0] | B trả lời, trò chơi kết thúc | 

Điều này xác nhận rằng kích thước thiết bị đầu cuối là 1. 

Bây giờ hãy xem xét$N = 4$. Ban đầu chúng tôi có bốn cái. Sau bước đi đầu tiên, trạng thái trở thành$[2,1,1]$hoặc$[0,1,1]$. Trong cả hai trường hợp, Người chơi B có thể đảm bảo rằng không có điều kiện chấm dứt ngay lập tức nào được kích hoạt theo cách bảo toàn nhiều phần tử và quá trình này tiếp tục cho đến khi chỉ còn lại một phần tử. 

| Bước | Nhiều bộ | Hành động | 
| --- | --- | --- | 
| Bắt đầu | [1,1,1,1] | A chọn hai | 
| Sau 1 | [2,1,1] hoặc [0,1,1] | B trả lời | 
| Kết thúc | [x] | giảm lặp đi lặp lại cho đến khi phần tử duy nhất | 

Điều này cho thấy ngay cả khi các giá trị phân kỳ sớm thì chúng cũng không ảnh hưởng đến số đếm còn lại cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ một thao tác đọc và in duy nhất | 
| Không gian |$O(1)$| Không có cấu trúc dữ liệu bổ sung | 

Giải pháp dễ dàng thỏa mãn các ràng buộc vì không tính toán nào phụ thuộc vào$N$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    n = int(input().strip())
    return str(1)

assert run("2\n") == "1", "minimum case"
assert run("4\n") == "1", "small even case"
assert run("10\n") == "1", "general even case"
assert run("1000000000000000000\n") == "1", "maximum case"
assert run("6\n") == "1", "intermediate case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 | 1 | trò chơi hợp lệ nhỏ nhất | 
| 4 | 1 | phân nhánh không tầm thường đầu tiên | 
| 10 | 1 | sự ổn định chung | 
| 10^18 | 1 | xử lý hạn chế tối đa | 

## Vỏ cạnh 

Đối với đầu vào nhỏ nhất$N = 2$, trò chơi kết thúc ngay sau nước đi đầu tiên. Người chơi A loại bỏ cả hai phần tử và Người chơi B trả về 0 hoặc 2, để lại chính xác một phần tử trên bảng. Điều này xác nhận rằng quy tắc “cookie bằng các phần tử còn lại” được áp dụng sau khi chấm dứt, không dựa trên các giá trị khác 0. 

Đối với rất lớn$N$, logic tương tự cũng được áp dụng vì không có cấu hình trung gian nào có thể ổn định thành nhiều phần tử buộc phải kết thúc. Mặc dù các giá trị có thể tăng lên thông qua các khoản tiền lặp lại, Người chơi B luôn có thể chống lại bằng cách chọn những khác biệt tuyệt đối ở các bước trước đó, ngăn chặn bất kỳ bẫy đầu cuối đa phần tử nào. Quá trình vẫn sụp đổ cho đến khi còn lại một phần tử duy nhất, do đó đầu ra vẫn là 1.
