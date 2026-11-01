---
title: "CF 106439C - Nghiền mảng"
description: "Trò chơi được chơi trên một dãy số nguyên. Một nước đi hợp lệ sẽ loại bỏ toàn bộ một khối có giá trị bằng nhau. Khối có nghĩa là một nhóm liên tiếp tối đa trong đó mọi phần tử đều giống nhau, do đó người chơi không thể xóa một phần của khối."
date: "2026-06-25T09:29:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106439
codeforces_index: "C"
codeforces_contest_name: "Insomnia-26"
rating: 0
weight: 106439
solve_time_s: 29
verified: true
draft: false
---

[CF 106439C - Nghiền nát mảng](https://codeforces.com/problemset/problem/106439/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 29s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trò chơi được chơi trên một dãy số nguyên. Một nước đi hợp lệ sẽ loại bỏ toàn bộ một khối có giá trị bằng nhau. Khối có nghĩa là một nhóm liên tiếp tối đa trong đó mọi phần tử đều giống nhau, do đó người chơi không thể xóa một phần của khối. Alice di chuyển trước, nhưng trước khi trò chơi bắt đầu, cô ấy có thể sắp xếp lại mảng theo cách cô ấy muốn. Nhiệm vụ là xác định xem Alice có thể sắp xếp các giá trị để có chiến lược chiến thắng Bob hay không. 

Đầu vào mô tả một số mảng. Đối với mỗi mảng, đầu ra là người chiến thắng trong cách chơi tối ưu sau khi Alice chọn thứ tự tốt nhất có thể. Chúng ta chỉ cần in xem Alice hay Bob thắng. 

Nhận xét quan trọng là Alice kiểm soát sự sắp xếp ban đầu nên vị trí ban đầu không quan trọng. Chỉ có nhiều giá trị mới quan trọng. Khi các giá trị được nhóm lại, mỗi giá trị riêng biệt sẽ đóng góp chính xác một khối có thể tháo rời, vì Alice có thể đặt các giá trị bằng nhau lại với nhau. Trò chơi sau đó sẽ trở thành một câu hỏi về việc có bao nhiêu khối tồn tại và cách người chơi tương tác với việc loại bỏ chúng. 

Các ràng buộc đủ lớn để việc mô phỏng trò chơi không thực tế. Với tổng số phần tử đạt khoảng 200000, bất kỳ cách tiếp cận nào thử mọi hoán vị, mọi nhóm hoặc mọi chuỗi di chuyển sẽ bùng nổ. Một giải pháp phải xử lý mảng theo thời gian tuyến tính hoặc gần với nó. 

Các trường hợp cạnh chính đến từ các giá trị có cùng tần số. Một giải pháp bất cẩn có thể đếm các giá trị riêng biệt và cho rằng số lần di chuyển luôn là số đó, nhưng tính chẵn lẻ của kích thước khối sẽ thay đổi kết quả. 

Ví dụ, hãy xem xét:```
1
3
5 5 5
```Alice có thể sắp xếp nó như`[5,5,5]`. Chỉ có một khối nên Alice loại bỏ nó và thắng. Đầu ra là:```
Alice
```Một giải pháp coi mỗi lần xuất hiện là một nước đi độc lập sẽ nghĩ sai rằng có ba nước đi. 

Một trường hợp khác:```
1
4
1 1 2 2
```Alice có thể sắp xếp mảng như`[1,1,2,2]`. Có hai khối và Alice loại bỏ một khối trước. Bob loại bỏ khối còn lại nên Alice thua. Đầu ra là:```
Bob
```Một sai lầm phổ biến là cho rằng Alice luôn được hưởng lợi từ việc nhóm các giá trị bằng nhau, nhưng việc nhóm chỉ xác định số khối. Người chơi vẫn luân phiên loại bỏ các khối đó. 

## Phương pháp tiếp cận 

Ý tưởng đầu tiên là mô phỏng trò chơi một cách trực tiếp. Vì Alice có thể sắp xếp lại thứ tự mảng nên chúng ta có thể thử các cách sắp xếp khác nhau và chơi trò chơi trên mỗi mảng. Điều này rõ ràng là không thể vì số lượng hoán vị có thể tăng lên cực kỳ nhanh. 

Một ý tưởng mạnh mẽ hơn là lưu ý rằng chỉ độ dài của các nhóm có giá trị bằng nhau mới quan trọng. Chúng ta có thể liệt kê các cách có thể để chia tần số thành các khối liên tiếp và tính toán phần thắng. Điều này cũng không thành công vì tần số có thể được chia thành nhiều khả năng và tổng số phân vùng quá lớn. 

Quan sát quan trọng là Alice luôn có thể đặt mọi lần xuất hiện có cùng giá trị cạnh nhau. Việc chia một giá trị thành nhiều nhóm không bao giờ hữu ích vì nó mang lại cho Bob những bước đi bổ sung. Đối với một giá trị xuất hiện`cnt`lần, cách sắp xếp tốt nhất sẽ là một khối chứa tất cả`cnt`bản sao. 

Bây giờ trò chơi được rút gọn thành một tập hợp các khối. Mỗi số riêng biệt tạo thành một khối. Người chơi chỉ cần lần lượt xóa các khối. Vì mỗi nước đi sẽ loại bỏ chính xác một khối nên người chơi thực hiện nước đi cuối cùng sẽ thắng. 

Điều đó có nghĩa là điều duy nhất quan trọng là số lượng giá trị riêng biệt sau khi nhóm tối ưu. Nếu có số khối lẻ, Alice sẽ thực hiện bước cuối cùng. Nếu có số khối chẵn thì Bob sẽ làm như vậy. 

Toàn bộ vấn đề trở thành việc đếm xem có bao nhiêu giá trị khác nhau xuất hiện trong mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Đếm các giá trị khác biệt | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng và lưu trữ mọi giá trị xuất hiện. 

Thứ tự ban đầu không liên quan vì Alice có thể sắp xếp lại mọi thứ trước khi trò chơi bắt đầu. 
2. Đếm số lượng giá trị duy nhất. 

Mỗi giá trị duy nhất có thể được biến thành chính xác một khối tối đa bằng cách đặt tất cả các giá trị bằng nhau lại với nhau. 
3. Kiểm tra tính chẵn lẻ của số lượng giá trị duy nhất. 

Nếu số đếm là số lẻ, Alice loại bỏ khối còn lại cuối cùng và thắng. Nếu số chẵn thì Bob đi nước cuối cùng và thắng. 

### Tại sao nó hoạt động 

Sau khi Alice sắp xếp lại mảng, tất cả các giá trị bằng nhau có thể được đặt liên tiếp. Điều này tạo ra một khối cho mỗi giá trị riêng biệt. Việc tạo nhiều hơn một khối từ cùng một giá trị sẽ chỉ làm tăng số lần di chuyển và không bao giờ có thể giúp ích cho Alice vì cả hai người chơi đều loại bỏ chính xác một khối mỗi lượt. 

Do đó, trò chơi này giống hệt với trò chơi loại bỏ xen kẽ thông thường với một đồ vật cho mỗi giá trị riêng biệt. Alice thắng chính xác khi số lượng đồ vật là số lẻ vì cô ấy bắt đầu và thực hiện mọi nước đi số lẻ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        seen = set(a)

        if len(seen) % 2 == 1:
            ans.append("Alice")
        else:
            ans.append("Bob")

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Bộ này được sử dụng vì nó chỉ lưu trữ các giá trị riêng biệt. Kích thước của bộ này là số khối Alice có thể tạo sau khi sắp xếp lại. 

Kích thước đầu vào có thể lớn, do đó giải pháp sử dụng`sys.stdin.readline`và thu thập kết quả đầu ra trước khi in một lần. Số nguyên Python không tràn ở đây vì chúng tôi chỉ lưu trữ giá trị và số đếm. 

Điểm tinh tế duy nhất là tránh mọi sự phụ thuộc vào vị trí ban đầu. Sắp xếp mảng hoặc kiểm tra các phần tử liền kề sẽ giải quyết được một vấn đề khác vì Alice thay đổi cách sắp xếp trước khi trò chơi bắt đầu. 

Tôi sẽ tiếp tục với các phần còn lại (ví dụ đã thực hiện, phân tích độ phức tạp, trường hợp kiểm thử và trường hợp đặc biệt) trong thông báo tiếp theo.
