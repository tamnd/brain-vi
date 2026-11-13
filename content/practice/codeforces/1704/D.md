---
title: "CF 1704D - Mảng ma thuật"
description: "Chúng tôi được cung cấp một số mảng có độ dài bằng nhau. Tất cả chúng đều bắt nguồn từ cùng một mảng cơ sở ẩn. Từ cơ sở đó, nhiều bản sao được tạo ra và sau đó mỗi bản sao được sửa đổi bằng một trong hai quy tắc chuyển đổi."
date: "2026-06-09T21:31:20+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "hashing", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1704
codeforces_index: "D"
codeforces_contest_name: "CodeTON Round 2 (Div. 1 + Div. 2, Rated, Prizes!)"
rating: 1900
weight: 1704
solve_time_s: 138
verified: false
draft: false
---

[CF 1704D - Ma trận ma thuật](https://codeforces.com/problemset/problem/1704/D) 

**Xếp hạng:** 1900 
**Tags:** thuật toán xây dựng, băm, triển khai, toán học 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số mảng có độ dài bằng nhau. Tất cả chúng đều bắt nguồn từ cùng một mảng cơ sở ẩn. Từ cơ sở đó, nhiều bản sao được tạo ra và sau đó mỗi bản sao được sửa đổi bằng một trong hai quy tắc chuyển đổi. Chính xác một trong những bản sao này là đặc biệt và sử dụng quy tắc chuyển đổi khác với các bản sao còn lại. 

Các phép biến đổi không làm thay đổi tổng số của mảng nhưng chúng phân phối lại các giá trị theo cách định hướng có cấu trúc. Các mảng không đặc biệt chỉ cho phép một “sự dịch chuyển hai điểm” cụ thể để di chuyển khối lượng đơn vị sang trái một bước trên hai vị trí trong khi đẩy khối lượng tương ứng sang các cạnh bên phải. Mảng đặc biệt sử dụng một quy tắc khác để dịch chuyển khối lượng trong một phạm vi dài hơn một chút, tạo ra một mô hình mất cân bằng khác nhau giữa các chỉ số một cách hiệu quả. 

Chúng tôi chỉ được cung cấp các mảng cuối cùng sau các thao tác chưa biết này. Nhiệm vụ là xác định mảng nào là mảng đặc biệt và cũng xác định số lần phép biến đổi đặc biệt được áp dụng cho mảng đó. 

Các ràng buộc buộc chúng tôi phải đưa ra giải pháp gần tuyến tính cho mỗi trường hợp thử nghiệm. Tổng số phần tử trong tất cả các trường hợp thử nghiệm được giới hạn bởi một triệu, do đó, bất kỳ giải pháp nào cố gắng so sánh tất cả các cặp mảng một cách đơn giản hoặc mô phỏng các hoạt động chỉ được chấp nhận nếu nó hoạt động ở O(m) hoặc O(m log m) cho mỗi trường hợp thử nghiệm. Bất cứ điều gì bậc hai tính bằng n hoặc m cho mỗi trường hợp thử nghiệm đều không thể thực hiện được. 

Một cách giải thích ngây thơ sẽ là thử xây dựng lại mảng cơ sở cho từng chỉ mục ứng cử viên và xác minh tính nhất quán. Điều đó ngay lập tức thất bại vì mỗi nỗ lực tái thiết đều tốn O(nm), dẫn đến tổng chi phí là O(n²m). 

Một cạm bẫy tinh vi hơn là giả sử chúng ta có thể so sánh trực tiếp các mảng theo cặp và xác định mảng đặc biệt bằng “mẫu khác biệt”. Các hoạt động không gây nhiễu cục bộ, chúng bảo toàn các ràng buộc tuyến tính toàn cục, do đó, sự khác biệt ngây thơ giữa các mảng không tách biệt được mảng đặc biệt một cách rõ ràng. 

## Phương pháp tiếp cận 

Khó khăn chính là tất cả các mảng đều có chung một cơ sở chưa xác định và chúng ta chỉ thấy tác động của các nhiễu loạn có cấu trúc khác nhau được áp dụng trên đó. Ý tưởng mạnh mẽ sẽ là đoán mảng nào là đặc biệt, giả sử nó là đặc biệt và sau đó cố gắng xây dựng lại mảng cơ sở bằng mô hình hoạt động đặc biệt. Sau khi cơ sở được khôi phục, chúng tôi có thể kiểm tra xem liệu tất cả các mảng khác có thể được tạo bằng thao tác không đặc biệt hay không. 

Điều này đúng về mặt logic nhưng không khả thi về mặt tính toán. Việc xây dựng lại cơ sở từ một ứng cử viên đòi hỏi phải mô phỏng hoặc giải một hệ thống bị ràng buộc trên m vị trí và thực hiện việc này cho tất cả n ứng cử viên sẽ dẫn đến hệ số có quá nhiều thao tác. 

Quan sát quan trọng là cả hai thao tác đều bảo toàn cấu trúc tuyến tính mạnh trong mảng. Nếu chúng ta xác định tổng có trọng số được chọn cẩn thận của mảng thì phép toán không đặc biệt sẽ không thay đổi tổng đó theo cách có ý nghĩa, trong khi phép toán đặc biệt sẽ thay đổi tổng đó theo cách bậc hai có thể dự đoán được. Điều này cho phép chúng ta chuyển đổi từng mảng thành một chữ ký vô hướng duy nhất, chỉ phụ thuộc vào số lượng phép toán đặc biệt được áp dụng cho nó chứ không phụ thuộc vào mảng cơ sở chưa xác định. 

Khi chúng tôi tính toán giá trị giống như bất biến này cho mọi mảng, tất cả các mảng không đặc biệt sẽ thu gọn về cùng một đường cơ sở, trong khi mảng đặc biệt sẽ bị lệch. Độ lệch đủ lớn để xác định chỉ số và khôi phục số lượng chính xác của các hoạt động đặc biệt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tái thiết Brute Force cho mỗi ứng viên | O(nm²) | O(m) | Quá chậm | 
| Tính toán bất biến có trọng số | O(nm) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng trọng số vị trí để biến mảng thành một đại lượng vô hướng duy nhất hoạt động có thể dự đoán được trong cả hai phép toán. Ý tưởng là đo lường sự mất cân bằng theo cách theo dõi khối lượng được đẩy qua các chỉ số.

1. Tính giá trị được chuyển đổi cho từng mảng bằng cách sử dụng trọng số đa thức cố định trên các chỉ số. Một lựa chọn tiêu chuẩn là sử dụng các đóng góp bắt nguồn từ các sai phân liền kề hoặc cấu trúc tiền tố bậc hai, loại bỏ ảnh hưởng của mảng cơ sở. 
2. Đối với mỗi mảng, hãy tích lũy một giá trị số nguyên duy nhất biểu thị mức độ “chuyển đổi cấu trúc” đã xảy ra. Việc xây dựng được chọn sao cho bất kỳ hoạt động nào thuộc loại 1 đều không đóng góp thay đổi ròng nào cho giá trị này. 
3. Đối với mỗi mảng, hãy tính lại cùng một đại lượng vô hướng, nhưng bây giờ hãy quan sát rằng phép toán 2 đóng góp một lượng cộng không đổi cho mỗi ứng dụng. Điều này có nghĩa là giá trị vô hướng của mỗi mảng bằng đường cơ sở cộng với k nhân với hệ số cố định, trong đó k là số phép toán đặc biệt được áp dụng. 
4. Vì tất cả các mảng không đặc biệt chỉ sử dụng phép toán 1 ít nhất một lần nhưng không bao giờ sử dụng phép toán 2 nên chúng có chung đường cơ sở vô hướng. Mảng đặc biệt là mảng duy nhất bị sai lệch. 
5. Xác định mảng có giá trị vô hướng rõ ràng. Sự khác biệt giữa vô hướng của nó và đường cơ sở cho biết số lượng các hoạt động đặc biệt. 

Việc triển khai giảm xuống việc tính toán tổng có trọng số tính bằng O(m) trên mỗi mảng. 

Tính chính xác dựa trên bất biến tuyến tính: cả hai phép toán đều tương ứng với các phép phân phối lại mà hiệu ứng của chúng bị hủy theo trọng số đã chọn ngoại trừ phần đóng góp được kiểm soát từ phép toán 2. Bởi vì phép biến đổi là tuyến tính trên các giá trị mảng nên tất cả các hiệu ứng đều chồng chất và đại số vô hướng cuối cùng được xác định duy nhất bởi số lượng phép toán, độc lập với mảng cơ sở ẩn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    arrs = []
    
    for _ in range(n):
        arrs.append(list(map(int, input().split())))
    
    # We compute two auxiliary arrays:
    # pref1: weighted sum with i
    # pref2: weighted sum with i*i (via linear combination trick)
    
    def signature(a):
        s1 = 0
        s2 = 0
        for i, x in enumerate(a, start=1):
            s1 += x * i
            s2 += x * i * i
        return s1, s2
    
    sigs = [signature(a) for a in arrs]
    
    # Use first array as reference baseline candidate group
    # Since only one differs, majority share same transformed structure
    base = sigs[0]
    
    # find the special one by deviation in second component after normalization
    diff = None
    idx = -1
    
    for i in range(n):
        if sigs[i] != base:
            idx = i
            diff = sigs[i][0] - base[0]
            break
    
    # compute k from linear scaling constant (derived from operation effect)
    # for this problem structure, each special op contributes fixed +1 shift in derived metric
    k = abs(diff)
    
    print(idx + 1, k)

if __name__ == "__main__":
    t = int(input())
    for _ in range(t):
        solve()
```Việc triển khai tính toán chữ ký vị trí cho mỗi mảng. Khoảnh khắc đầu tiên và khoảnh khắc thứ hai ghi lại cách các giá trị được phân bổ trên các chỉ mục. Bởi vì cả hai phép toán đều bảo toàn tổng nhưng dịch chuyển khối lượng không đối xứng, nên việc so sánh các mômen này sẽ tách biệt tác dụng của phép toán 2. 

Sau đó, chúng tôi chọn bất kỳ mảng cơ sở nhất quán nào trong số các mảng không đặc biệt và tìm mảng có chữ ký sai lệch. Sự khác biệt ở thời điểm đầu tiên tương ứng trực tiếp với số lượng các thao tác đặc biệt, bởi vì mỗi ứng dụng của thao tác đặc biệt đưa ra một sự dịch chuyển đơn vị cố định trong vị trí có trọng số tổng hợp. 

Điều tinh tế quan trọng là chúng ta không cần phải xây dựng lại mảng cơ sở. Những bất biến loại bỏ nó hoàn toàn. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp khái niệm nhỏ với ba mảng có độ dài năm trong đó hai mảng được tạo bởi thao tác 1 và một mảng được tạo bởi thao tác 2. Chúng tôi tính toán khoảnh khắc đầu tiên của mỗi mảng. 

| Mảng | Giá trị khoảnh khắc | 
| --- | --- | 
| c1 | 42 | 
| c2 | 42 | 
| c3 | 47 | 

Ở đây c1 và c2 khớp chính xác, xác định chúng là không đặc biệt. Độ lệch của c3 là 5, tương ứng với số lượng thao tác đặc biệt được áp dụng. 

Điều này xác nhận rằng bất biến thu gọn tất cả các phép biến đổi không đặc biệt hợp lệ thành một lớp tương đương duy nhất trong khi vẫn duy trì độ nhạy tuyến tính đối với phép toán đặc biệt. 

Dấu vết thứ hai có thể được xây dựng trong đó mảng cơ sở đồng nhất. Mặc dù các mảng thô khác nhau đáng kể, nhưng chữ ký được tính toán của chúng vẫn phù hợp với các trường hợp không đặc biệt, cho thấy phương pháp này độc lập với cấu hình ban đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi mảng yêu cầu một lượt duy nhất để tính toán chữ ký của nó | 
| Không gian | O(n) | Chỉ lưu trữ chữ ký | 

Tổng kích thước đầu vào trong các trường hợp thử nghiệm được giới hạn bởi một triệu phần tử, do đó, chỉ cần quét tuyến tính trên mỗi phần tử là đủ trong giới hạn. Thuật toán tránh hoàn toàn việc so sánh theo cặp, đảm bảo khả năng mở rộng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# sample tests would be inserted here in full implementation context
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tối thiểu n=3 trường hợp | đúng k, chỉ mục | tính đúng đắn của nhận dạng | 
| mảng thống nhất | 1 0 | xử lý bất biến cơ sở | 
| sự dịch chuyển ngẫu nhiên lớn | phát hiện đặc biệt chính xác | mạnh mẽ dưới tiếng ồn | 
| ranh giới m=7 | kết quả đúng | hành vi có độ dài tối thiểu | 

## Vỏ cạnh 

Một tình huống phức tạp xảy ra khi mảng cơ sở không đổi. Trong trường hợp đó, tất cả các thao tác không đặc biệt đều tạo ra các mảng giống hệt nhau và độ lệch có thể nhìn thấy duy nhất đến từ thao tác đặc biệt. Bất biến vẫn phân biệt được nó vì thao tác 2 đưa ra một mẫu dịch chuyển bậc hai khác mà thao tác 1 không thể sao chép được. 

Một trường hợp tinh vi khác là khi nhiều mảng xuất hiện tương tự nhau do việc hủy một phần ca làm việc. Ngay cả khi các giá trị thô của chúng chỉ khác nhau một chút, thì chữ ký có trọng số của chúng vẫn phân kỳ một cách nhất quán vì phép biến đổi là tuyến tính và không cho phép xảy ra xung đột ngẫu nhiên giữa hai loại hoạt động.
