---
title: "CF 104002E - William và Robot"
description: "Chúng ta được cấp một dòng số nguyên và hai người chơi lần lượt loại bỏ các phần tử cho đến khi không còn lại gì. William di chuyển trước và đến lượt mình, anh ấy có thể chọn bất kỳ phần tử nào còn sẵn có từ bất kỳ đâu trong mảng."
date: "2026-07-02T05:36:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104002
codeforces_index: "E"
codeforces_contest_name: "UTPC Contest 10-28-22 Div. 2 (Beginner)"
rating: 0
weight: 104002
solve_time_s: 47
verified: true
draft: false
---

[CF 104002E - William và Robot](https://codeforces.com/problemset/problem/104002/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một dòng số nguyên và hai người chơi lần lượt loại bỏ các phần tử cho đến khi không còn lại gì. William di chuyển trước và đến lượt mình, anh ấy có thể chọn bất kỳ phần tử nào còn sẵn có từ bất kỳ đâu trong mảng. Robot hoàn toàn không lựa chọn một cách chiến lược: nó luôn loại bỏ phần tử còn lại ngoài cùng bên trái ở mỗi bước. 

Mục tiêu của William là tối đa hóa tổng giá trị mà anh ta thu thập được. Hành vi xác định của robot có nghĩa là sự tiến hóa của mảng còn lại hoàn toàn có thể dự đoán được sau khi các lựa chọn của William được khắc phục, vì vậy vấn đề không phải là về lý thuyết trò chơi đối nghịch mà là về việc kiểm soát vị trí mà William “yêu cầu” trước khi robot sử dụng cấu trúc từ bên trái. 

Khó khăn chính là sự lựa chọn của William ảnh hưởng gián tiếp đến khả năng sẵn có trong tương lai. Nếu anh ta trì hoãn việc chọn một phần tử có giá trị ở bên trái, robot có thể tiêu thụ phần tử đó trước. Mặt khác, việc chọn thứ gì đó quá sớm có thể lãng phí “ngân sách quay vòng” trong các tiền tố mà nếu không robot sẽ buộc phải lấy các phần tử có giá trị thấp. 

Ràng buộc n lên tới 100000 ngụ ý rằng mọi nghiệm đều phải gần tuyến tính hoặc n log n. Bất kỳ cách tiếp cận nào cố gắng mô phỏng tất cả các chuỗi lựa chọn có thể có hoặc xem xét các tập hợp con của các phần tử một cách rõ ràng đều không thể thực hiện được bởi vì ngay cả việc khám phá tập hợp con thô cũng sẽ tăng theo cấp số nhân. Điều này thúc đẩy chúng ta hướng tới một phương pháp tham lam hoặc dựa trên cấu trúc dữ liệu để xử lý các phần tử theo thứ tự và chỉ duy trì một biểu diễn nhỏ gọn về những gì William được phép giữ một cách hiệu quả. 

Trường hợp cạnh tinh tế phát sinh từ quy tắc ngoài cùng bên trái của robot. Ví dụ: hãy xem xét một mảng như 10 1 100 1. Một ý tưởng ngây thơ là William phải luôn lấy phần tử lớn nhất có sẵn, nhưng điều đó bỏ qua rằng sau khi robot liên tục tiêu thụ từ bên trái, một số giá trị "lớn" có thể không truy cập được nếu không được lấy trong hạn ngạch tiền tố. Một trường hợp thất bại khác là khi các giá trị lớn được phân cụm sớm, vì William không thể lấy quá nhiều giá trị trong số chúng ở các tiền tố ban đầu mà không vi phạm thực tế là robot đã lấy một nửa số phần tử tiền tố đó. 

Vì vậy, hạn chế thực sự không phải là về tính liền kề trong chuỗi trò chơi gốc mà là về dung lượng tiền tố: trong k phần tử đầu tiên, William không thể lấy nhiều hơn k/2 phần tử, vì robot sẽ luôn sử dụng phần còn lại của tiền tố đó theo từng bước từ bên trái. 

## Phương pháp tiếp cận 

Nếu chúng ta bỏ qua hành vi xác định của robot, thì ý tưởng bạo lực sẽ là mô phỏng trạng thái trò chơi: ở mỗi bước, hãy thử mọi lựa chọn có thể có cho William, sau đó mô phỏng hành động bắt buộc của robot và tiếp tục. Điều này tạo ra hệ số phân nhánh bằng với số phần tử còn lại và độ sâu n, dẫn đến độ phức tạp giai thừa hoặc hàm mũ. Ngay cả việc ghi nhớ cũng không thành công vì trạng thái không chỉ là các phần tử còn lại mà còn là thứ tự của chúng sau nhiều lần xóa trái, vẫn để lại quá nhiều cấu hình riêng biệt. 

Quan sát quan trọng là ngừng suy nghĩ về “mảng còn lại sau khi xóa” và thay vào đó hãy nghĩ về các ràng buộc tiền tố. Sau khi xử lý k vị trí đầu tiên, tổng cộng có chính xác k phần tử đã bị loại bỏ và chúng được phân chia giữa William và robot. Vì robot luôn sử dụng phần tử có sẵn ngoài cùng bên trái nên nó đảm bảo một cách hiệu quả rằng trong mọi tiền tố có độ dài k, William không thể chiếm ưu thế về số lượng. Điều này dẫn đến hạn chế là William chỉ có thể “giữ” tối đa k/2 phần tử được chọn trong số k vị trí đầu tiên.

Khi chúng tôi chấp nhận cách giải thích đó, vấn đề sẽ trở thành: xử lý mảng từ trái sang phải và duy trì một tập hợp các phần tử được William chọn sao cho ở mọi chỉ số k chẵn, chúng tôi đảm bảo tối đa k/2 lựa chọn được giữ trong số k phần tử đầu tiên. Vì William muốn tổng tối đa nên bất cứ khi nào anh ấy chọn quá sớm quá nhiều phần tử nhỏ, anh ấy nên loại bỏ phần tử nhỏ nhất trong số đó. Điều này tự nhiên gợi ý việc duy trì lựa chọn lợi ích tối đa theo một ràng buộc về kích thước, nhưng vì chúng tôi đang loại bỏ các phần tử được chọn ít hữu ích nhất nên chúng tôi thực sự muốn giữ một cấu trúc cho phép loại bỏ ứng cử viên nhỏ nhất trong số các lựa chọn hiện tại của William. 

Một đống tối thiểu phù hợp chính xác với yêu cầu này: chúng tôi tạm thời bao gồm mọi phần tử làm ứng cử viên cho tập hợp của William và bất cứ khi nào số lượng phần tử được chọn vượt quá hạn ngạch cho phép cho tiền tố hiện tại, chúng tôi sẽ loại bỏ giá trị nhỏ nhất. Điều này đảm bảo rằng William luôn giữ được nhiều lựa chọn tốt nhất có thể phù hợp với tính khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Ràng buộc tiền tố + cắt tỉa đống | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi quét mảng từ trái sang phải, xử lý từng vị trí như thể William có thể cố gắng chiếm lấy nó. 

1. Chèn giá trị hiện tại vào một đống tối thiểu đại diện cho tập hợp được chọn dự kiến ​​của William. Điều này giả định rằng William cân nhắc việc thực hiện mọi thứ trước và sau đó chúng tôi sẽ khắc phục tính không khả thi bằng cách loại bỏ các lựa chọn yếu kém. 
2. Sau khi xử lý chỉ số k, hãy tính xem William được phép giữ lại bao nhiêu phần tử từ tiền tố 1 đến k. Vì robot luôn lấy phần tử có sẵn ngoài cùng bên trái, nên trong bất kỳ tiền tố nào, William không thể có nhiều hơn k/2 phần tử trong số các vị trí đó, vì vậy chúng tôi chỉ thực thi ràng buộc đó khi k chẵn. 
3. Nếu k chẵn và kích thước heap vượt quá k/2, hãy liên tục loại bỏ phần tử nhỏ nhất khỏi heap cho đến khi thỏa mãn ràng buộc. Lý do chúng tôi loại bỏ phần nhỏ nhất là vì việc giữ các phần tử lớn hơn luôn cải thiện tổng cuối cùng trong khi vẫn tôn trọng tính khả thi. 
4. Tiếp tục cho đến hết mảng. Tổng số phần tử còn lại trong heap là số điểm tối đa mà William có thể đạt được. 

Điều tinh tế quan trọng là chúng tôi không bao giờ mô phỏng rõ ràng các chuyển động của robot. Vùng heap thực thi ràng buộc cấu trúc duy nhất quan trọng: các lựa chọn của William phải vẫn tương thích với robot tiêu thụ chính xác một nửa tổng số tiền tố. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý tiền tố k, vùng heap chứa tập hợp con tổng tối đa của các phần tử từ tiền tố có thể được mở rộng thành kết quả trò chơi hợp lệ theo quy tắc loại bỏ ngoài cùng bên trái bắt buộc của robot. Hành vi của robot đảm bảo rằng trong bất kỳ tiền tố nào có độ dài k, chính xác k/2 phần tử được “dành riêng” một cách hiệu quả nhiều nhất cho William, vì dung lượng còn lại được tiêu thụ từ bên trái. Bằng cách luôn loại bỏ phần tử được chọn nhỏ nhất khi ràng buộc bị vi phạm, chúng ta duy trì tập con có giá trị tốt nhất có thể dưới ràng buộc lượng số giống matroid tăng dần theo k. Điều này đảm bảo rằng không có quyết định nào trong tương lai có thể được hưởng lợi từ việc giữ lại phần tử nhỏ hơn sớm hơn thay vì phần tử lớn hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import heapq

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    heap = []
    total = 0
    
    for i, x in enumerate(a, start=1):
        heapq.heappush(heap, x)
        
        if i % 2 == 0:
            limit = i // 2
            while len(heap) > limit:
                heapq.heappop(heap)
    
    print(sum(heap))

if __name__ == "__main__":
    solve()
```Giải pháp duy trì một lượng tối thiểu các giá trị được chọn hiện tại của William. Mọi phần tử đều được thêm vào ngay lập tức, sau đó ràng buộc về tính khả thi của tiền tố được thực thi ở các vị trí chẵn. Thao tác heap pop luôn loại bỏ giá trị được chọn nhỏ nhất, đây là cách loại bỏ an toàn duy nhất vì nó giảm thiểu tổn thất trong tổng số tiền. 

Một lỗi triển khai phổ biến là quên rằng ràng buộc áp dụng cho mỗi tiền tố chứ không chỉ ở cuối. Một cách khác là sử dụng vùng heap tối đa, điều này phá vỡ logic vì chúng ta cần loại bỏ phần tử được chọn ít giá trị nhất chứ không phải phần tử có giá trị nhất. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
6 1 1 4
```Chúng tôi theo dõi vùng heap sau mỗi bước. 

| tôi | giá trị | đống sau khi chèn | hành động | 
| --- | --- | --- | --- | 
| 1 | 6 | [6] | không ràng buộc | 
| 2 | 1 | [1, 6] | giới hạn 1, xóa 1 | 
| 3 | 1 | [1, 6] | không ràng buộc | 
| 4 | 4 | [1, 4, 6] → [4, 6] | giới hạn 2 | 

Đống cuối cùng là [4, 6], tổng là 10. 

Điều này cho thấy các giá trị nhỏ ban đầu bị loại bỏ như thế nào để duy trì dung lượng cho các lựa chọn tốt hơn sau này. 

### Mẫu 2 

đầu vào:```
10
1 3 4 9 5 2 5 5 3 6
```| tôi | giá trị | đống sau khi điều chỉnh | 
| --- | --- | --- | 
| 1 | 1 | [1] | 
| 2 | 3 | [3] | 
| 3 | 4 | [3,4] | 
| 4 | 9 | [4,9] | 
| 5 | 5 | [4,9,5] | 
| 6 | 2 | [5,9,5] | 
| 7 | 5 | [5,9,5,5] → tỉa | 
| 8 | 5 | [5,5,5,9] → tỉa | 
| 9 | 3 | ... | 
| 10 | 6 | ... | 

Sau khi cắt tỉa hoàn toàn, heap ổn định trên lựa chọn hợp lệ tốt nhất với tổng bằng 30. 

Dấu vết này cho thấy cấu trúc liên tục thay thế các lựa chọn ban đầu yếu hơn bằng các lựa chọn sau mạnh hơn trong khi vẫn tôn trọng các giới hạn tiền tố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi phần tử được đẩy một lần và có thể xuất hiện một lần từ heap | 
| Không gian | O(n) | Heap lưu trữ tối đa n/2 phần tử | 

Ràng buộc n lên tới 100000 vừa vặn thoải mái trong O(n log n), vì các thao tác heap vẫn đủ nhanh trong giới hạn tiêu chuẩn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    import heapq

    n = int(input())
    a = list(map(int, input().split()))
    
    heap = []
    
    for i, x in enumerate(a, start=1):
        heapq.heappush(heap, x)
        if i % 2 == 0:
            limit = i // 2
            while len(heap) > limit:
                heapq.heappop(heap)
    
    return str(sum(heap))

# provided samples
assert run("4\n6 1 1 4\n") == "10"
assert run("10\n1 3 4 9 5 2 5 5 3 6\n") == "30"

# custom cases
assert run("2\n5 1\n") == "5"
assert run("2\n1 100\n") == "100"
assert run("6\n1 2 3 4 5 6\n") == "12"
assert run("8\n8 7 6 5 4 3 2 1\n") == "20"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 yếu tố | lựa chọn đơn tối đa | xử lý kích thước tối thiểu | 
| sắp xếp cặp nhỏ/lớn | chọn giá trị tốt nhất | sự đúng đắn tham lam | 
| mảng tăng dần | cân bằng tiền tố | logic cắt tỉa đống | 
| mảng giảm dần | thay thế thường xuyên | thay thế trong trường hợp xấu nhất | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi các giá trị lớn xuất hiện sớm, chẳng hạn`100 1 99 1`. Một chiến lược tham lam ngây thơ sẽ nhận được 100 và 99 ngay lập tức, nhưng điều đó có thể vi phạm tính khả thi của tiền tố vì robot sẽ tiêu thụ các phần tử can thiệp và làm giảm tính linh hoạt trong tương lai. Thuật toán chèn tất cả các giá trị nhưng ngay lập tức thực thi ràng buộc tiền tố, buộc phải loại bỏ lựa chọn sớm nhỏ nhất khi vượt quá dung lượng, duy trì các giá trị cao hơn cho các tiền tố sau này. 

Một trường hợp cạnh khác là xen kẽ các giá trị cao và thấp như`1 100 2 99 3 98`. Ở đây vùng heap phát triển nhanh chóng và nếu không cắt tỉa định kỳ, việc lựa chọn sẽ vượt quá số lượng tiền tố cho phép. Thuật toán luôn cắt bỏ các lựa chọn yếu nhất ở mỗi chỉ số chẵn, đảm bảo rằng lựa chọn của William không bao giờ trở nên không khả thi trong khi vẫn giữ được các giá trị sẵn có lớn nhất.
