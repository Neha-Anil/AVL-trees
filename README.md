# AVL-trees
for each input ,the tree balances itself where the difference of the height between left and right sub-trees is either -1,0 or 1
#include<stdio.h>
#include<stdlib.h>
#include<math.h>
typedef struct NODE
{
        int data;
        struct NODE *llink;
        struct NODE *rlink;
}NODE;
NODE* getnode()
{
        NODE *temp;
        temp=(NODE *)malloc(sizeof(NODE));
        if(temp==NULL)
        {
                printf("\n NO MEMORY.");
                return;
        }
        temp->llink=temp->rlink=NULL;
        return temp;
}
int findheight(NODE *root)
{       
        int l,r;
        if(root==NULL)
                return -1;
        l=findheight(root->llink);
        r=findheight(root->rlink);
        if(l>r)
                return (l+1);
        else
                return (r+1);
}
NODE* findmin(NODE *temp)
{
        NODE *min=temp;
        int ele=temp->data;
        while(temp->rlink!=NULL)
        {
                temp=temp->rlink;
                if(ele>temp->data)
                {
                        ele=temp->data;
                        min=temp;
                }
        }
        return min;
}
int search(int ele,NODE *root)
{
        if(root!=NULL)
        {

                if(root->data==ele)
                        return ele;
                if(ele<(root->data))
                        return search(ele,(root->llink));
                return search(ele,(root->rlink));
        }
        return -1;
}
NODE* left_rotate(NODE *t)
{
        NODE *temp;
        temp=t->rlink;
        t->rlink=temp->llink;
        temp->llink=t;
        return temp;
}
NODE* right_rotate(NODE *t)
{
        NODE *temp;
        temp=t->llink;
        t->llink=temp->rlink;
        temp->rlink=t;
        return temp;

}
NODE* leftright_rotate(NODE *t)
{
        t->llink=left_rotate(t->llink);
        t=right_rotate(t);
        return t;
}
NODE* rightleft_rotate(NODE *t)
{
        t->rlink=right_rotate(t->rlink);
        t=left_rotate(t);
        return t;
}
int bal_factor(NODE *root)
{
        if (root == NULL)
                return 0;
        return (findheight(root->llink) - findheight(root->rlink));
}
NODE* balance(NODE *root)
{
        int dif=bal_factor(root);
        if(dif>1)
        {
                if(bal_factor(root->llink)>0)
                        root=right_rotate(root);
                else
                        root=leftright_rotate(root);
        }
        else if(dif<-1)
        {
                if(bal_factor(root->rlink)>0)
                        root=rightleft_rotate(root);
                else
                        root=left_rotate(root);
        }
        return root;
}
NODE* ins(int ele,NODE *root)
{

        NODE *new=getnode();
        new->data=ele;
        new->rlink=new->llink=NULL;
        if(root==NULL)
        {
                return new;
        }
        if(ele<root->data)
        {
                root->llink=ins(ele,root->llink);
        }
        else if(ele>root->data)
        {
                root->rlink=ins(ele,root->rlink);

        }
        return root;
}
NODE* delete(int ele,NODE *root)
{
        if(root==NULL)
        {
                printf("\n EMPTY LIST.");
                return NULL;
        }
        else if(ele<root->data)
        {
                root->llink=delete(ele,root->llink);
                return root;
        }
        else if(ele>root->data)
        {
                root->rlink=delete(ele,root->rlink);
                return root;
        }
        else
        {
                if(root->llink==NULL && root->rlink==NULL)
                {
                        root==NULL;
                        return NULL;
                }
                else if(root->llink==NULL)
                {
                        NODE *temp=root;
                        root=root->rlink;
                        free(temp);
                        return root;
                }
                else if(root->rlink==NULL)
                {
                        NODE *temp=root;
                        root=root->llink;       
                        free(temp);
                        return root;
                }
                else
                {
                        NODE *temp;
                        temp=findmin(root->rlink);
                        root->data=temp->data;
                        root->rlink=delete(temp->data,root->rlink);

                }
        }
        return root;
}
void inorder(NODE *root)
{
        if(root)
        {
                inorder(root->llink);
                printf("%d \n",root->data);
                inorder(root->rlink);
        }

}
void preorder(NODE *root)
{
        if(root)
        {
                printf("%d \n",root->data);
                preorder(root->llink);
                preorder(root->rlink);
        }

}
void postorder(NODE *root)
{
        if(root)
        {
                postorder(root->llink);
                postorder(root->rlink);
                printf("%d \n",root->data);
        }

}
void main()
{       
        NODE *root=NULL;
        int choice,ele,flag;
        for(;;)
        {
                printf("\n Enter 1 to Insert an element.");
                printf("\n Enter 2 to Search an element.");
                printf("\n Enter 3 to Delete an element.");
                printf("\n Enter 4 for Inorder Traversal.");
                printf("\n Enter 5 for Preorder Traversal.");
                printf("\n Enter 6 for Postorder Traversal..");
                printf("\n Enter 7 to Exit.");
                scanf("%d",&choice);
                switch(choice)
                {
                        case 1:printf("\n Enter the element to be inserted:");
                                scanf("%d",&ele);
                                root=ins(ele,root);
                                root=balance(root);
                                break;
                        case 2:printf("\n Enter the element to be searched: ");
                                scanf("%d",&ele);
                                flag=search(ele,root);
                                if(flag==-1)
                                        printf("\n Element not found.\n");
                                else
                                        printf("\n Element found.\n");
                                break;
                        case 3:printf("\n Enter the element to be deleted: ");
                                scanf("%d",&ele);
                                root=delete(ele,root);
                                root=balance(root);
                                break;
                        case 4:if(root==NULL)
                                        printf("\n TREE EMPTY.");
                                else
                                {
                                        printf("\n The elements are:\n ");
                                        inorder(root);
                                }
                                break;
                        case 5:if(root==NULL)
                                        printf("\n TREE EMPTY.");
                                else
                                {
                                        printf("\n The elements are:\n ");
                                        preorder(root);
                                }
                                break;
                        case 6:if(root==NULL)
                                        printf("\n TREE EMPTY.");
                                else
                                {
                                        printf("\n The elements are:\n ");
                                        postorder(root);
                                }
                                break;
                        case 7:exit(0);
                }
        }
}
