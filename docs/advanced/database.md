# 数据库
# 使用 python3.7 自带的sqlite3 数据库
'''
选择理由：
      轻量级，简单，足以应对插件数据的存储
'''
import sqlite3

# 建表
# curs.execute('''  
#     create table stu (
#         id int primary key,
#         qq int,
#         data text,
#         dt date
#     )
# ''')


class ConnDb:
    def __init__(self):
        self.conn = sqlite3.connect("D:\\data\\QQmemo.db")  # 链接数据库
        self.curs = self.conn.cursor()  # 创建命令对象

    def add(self, qq, data, date):  # 插入数据
        data = '\''+data+'\''
        date = '\''+date+'\''
        vl = 'NULL,%d,%s, %s' % (qq, data, date)
        self.curs.execute('insert into Stu values(%s)' % vl)
        self.conn.commit()  # 事件提交
        return self.conn.total_changes

    def select(self, qq=742122071):
        self.curs.execute('''
        select id, memo, dt from stu
        where qq=%d
        ''' % qq)
        # self.conn.commit()  # 事件提交
        results = self.curs.fetchall()
        return results

    def close(self):
        self.conn.close()  # 关闭数据库

    def delete(self, id, qq):
        self.curs.execute("DELETE from Stu WHERE \
         id=%s and qq=%s" % (id, qq))  # 自己的只能删除自己的
        self.conn.commit()
        return self.conn.total_changes  # 删除了多少数据，成功删除返回 1， 失败返回 0


def init():  # 预留两个接口，方便在整个文件中访问到这个数据库接口
    global cn
    cn = ConnDb()
    print('数据库打开成功')


def get():
    return cn

# f = cn.add(3, 398161394, '起床了', '2019-4-1')
# print(f)
# # cn.select()
# # cn.delete(5)
# cn.close()  # 关闭数据库
